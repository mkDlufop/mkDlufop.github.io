---
layout: post
title: 任务控制（job control）
subtitle: 进程、用户进程控制、服务
tags: [Shell]
---

* TOC
{:toc}

## 1.进程

### 进程标识符

进程标识符（ PID , Process Identifier ）是进程的唯一标识。

> Linux 进程启动顺序
>
> Linux 系统内核从引导程序接手控制权后，开始内核初始化，随后变为 init_task ，初始化自己的 PID 为 0 。随后创建 1 号进程（ init 程序，目前一般为 systemd ）衍生出用户空间的所有进程，创建 2 号进程 kthreadd 衍生出所有内核线程。
>
> top 命令（按 f 键可以选择把 PPID 列显示出来）中无法看到 0 号进程本体，只能发现 1 号和 2 号进程的 PPID 为 0 。
### 进程组织结构

进程父子关系

除了最开始的 0 号进程外，其他进程一定由另一个进程通过  fork() 产生，产生进程的一方为父进程，被产生的一方为子进程。

> 进程父子关系下有两种特殊的运行情况：
>
> 1，父进程先退出，它的子进程成为孤儿进程（ orphan ）。
>
> 2，子进程先退出，父进程未作出反应，子进程就会变成僵尸进程（ zombie ）。
>
> 
>
> 孤儿进程由操作系统回收，交给 init “领养”。
>
> 僵尸进程的进程资源大部分已释放，但占用一个 PID ，并保存返回值。系统中大量僵尸进程的存在将导致无法创建进程。

### 进程调度

PRI 和 nice

内核中针对进程有实时 ( realtime ) 和非实时调度方法，它们分别使用了 realtime priotiry ( PRI ) 和 nice ( NI ) 来表示优先级。

PRI 值越高表示优先级越高，nice 值越高表示优先级越低。

进程状态

进程状态可以分为：start、running、waiting/blocked、ready、terminated。



## 2.用户进程控制

### 信号

信号列表可以使用 `man 7 signal` 或 `kill -l` 或 [这里](https://en.wikipedia.org/wiki/Signal_(IPC)) 查看。

| 常用信号                        | 意义                               | 默认行为 | 产生手段                                                     |
| :------------------------------ | ---------------------------------- | -------- | ------------------------------------------------------------ |
| SIGINT（interrupt）             | 朋友，别干了                       | 终止进程 | Ctrl+C                                                       |
| SIGTERM（termiate）             | 请优雅地死去（标准的终止进程信号） | 终止进程 | kill \<PID>       （ kill 后不加任何参数，将自动使用 SIGTERM 作为信号参数） |
| SIGKILL（kill）                 | 请立即去世                         | 终止进程 | kill -9 \<PID> 或 <br/>kill -9 %\<job number> |
| SIGSEGV（segment violation）    | 你想知道的太多了                   | 核心转储 | 什么都不用做，这是程序写得太烂的缘故                         |
| SIGSTOP（stop） SIGTSTP（stop） | 让某个进程变成植物人               | 停止进程 | Ctrl+Z                                                       |
| SIGCONT（continue）             | 让植物人苏醒                       | 继续进程 | fg   bg                                                      |

> `SIGKILL` 是一个特殊的信号，它不能被进程捕获并且它会马上结束该进程。不过这样做会有一些副作用，例如留下孤儿进程。

### 暂停和后台执行进程

例子：

```shell
$ sleep 1000
^Z
[1]  + 18653 suspended  sleep 1000

### 命令中的 & 后缀可以让命令直接在后台运行，不过它此时还是会使用 shell 的标准输出，
###（这种情况可以使用 shell 重定向处理）
$ nohup sleep 2000 &
[2] 18745
appending output to nohup.out

$ jobs
[1]  + suspended  sleep 1000
[2]  - running    nohup sleep 2000

### 直接执行 %1，会把 1 号进程转移到前台执行
$ bg %1
[1]  - 18653 continued  sleep 1000

$ jobs
[1]  - running    sleep 1000
[2]  + running    nohup sleep 2000

$ kill -STOP %1
[1]  + 18653 suspended (signal)  sleep 1000

$ jobs
[1]  + suspended (signal)  sleep 1000
[2]  - running    nohup sleep 2000

$ kill -SIGHUP %1
[1]  + 18653 hangup     sleep 1000

$ jobs
[2]  + running    nohup sleep 2000

$ kill -SIGHUP %2

$ jobs
[2]  + running    nohup sleep 2000

$ kill %2
[2]  + 18745 terminated  nohup sleep 2000

$ jobs
$
```

> 注意，后台的进程仍然是自己的终端进程的子进程，一旦您关闭终端（会发送另外一个信号 `SIGHUP` ），这些后台的进程也会终止。为了防止这种情况发生，您可以使用 [`nohup`](https://www.man7.org/linux/man-pages/man1/nohup.1.html) (一个用来忽略 `SIGHUP` 的封装) 来运行程序。针对已经运行的程序，可以使用 `disown` 。除此之外，还可以使用终端多路复用器来实现。

>**Exiting from a REPL (^D)**
>
>[如果使用的是 Python、Coin、Bash 或任何其他在运行时需要反复获得输入的程序，可以通过按下 ^D 来让程序退出。](https://www.cs.cmu.edu/~15131/f17/topics/terminal-usage/jobs-man-links/#:~:text=If%20you%E2%80%99re%20working%20in%20python%2C%20coin%2C%20bash%20or%20any%20other%20program%20that%20repeatedly%20gets%20input%20from%20you%20while%20it%E2%80%99s%20running%2C%20you%20can%20almost%20always%20make%20the%20program%20exit%20by%20pressing%20%5ED%20(or%20end%20of%20file%2C%20commonly%20abbreviated%20EOF).)
>
>[REPL ( read-eval-print loop )是一个简单的可交互的计算机编程环境。正如 REPL 的名字所说，它获取用户的输入，以某种方式评估输入，输出评估的结果，重复该过程。在 REPL 环境中编写的程序是分段执行的。](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop#:~:text=A%20read%E2%80%93eval%E2%80%93print%20loop%20(REPL)%2C%20also%20termed%20an%20interactive%20toplevel%20or%20language%20shell%2C%20is%20a%20simple%20interactive%20computer%20programming%20environment%20that%20takes%20single%20user%20inputs%2C%20executes%20them%2C%20and%20returns%20the%20result%20to%20the%20user%3B%20a%20program%20written%20in%20a%20REPL%20environment%20is%20executed%20piecewise.%5B1%5D)
>
>[这里的 ^D ( Ctrl + D ) 表示的是 EOF ( end of file )，可以通过 `stty -a` 查看到。](https://unix.stackexchange.com/questions/110240/why-does-ctrl-d-eof-exit-the-shell)

## 3.服务

### 守护进程

一直工作于后台的进程成为守护进程（ daemon ）。

### 服务管理

在 init 进程为 systemd 的系统中，管理系统服务的接口主要有 **systemctl** 和 **service** 两个命令。 service 命令主要是为了跨 init 系统的兼容性考虑，它的任务可以全部由 systemctl 完成。

service --status-all 可以查看目录 `/etc/init.d` 下的服务。

systemctl list-units 可以查看全部服务内容，它会显示所有 systemd 管理的单元。

> /etc/init.d 是在 systemd 作为 init 系统前放置服务的目录。出于向后兼容性的考虑，systemd 支持从此目录中加载服务。

### 自定义服务

例如自定义 a 服务：

1. 在 `/etc/systemd/system` 目录下创建一个名为 `aaa.service` 的文件，文件内容如下：

   ```shell
   [Unit]
   Description=aaa service    # 该服务简要描述
   
   [Service]
   PIDFile=/run/aaa.pid        # 用来存放 PID 的文件
   ExecStart=/usr/local/bin/aaa --allow-root  # 使用绝对路径标明的命令及选项
   WorkingDirectory=/root
   Restart=always                  # 重启模式，这里是无论因何退出都重启
   RestartSec=10                   # 退出后多少秒重启
   
   [Install]
   WantedBy=multi-user.target      # 依赖目标，这里指多用户模式启动后再启动该服务
   ```

2. 运行 `systemctl daemon-reload` 后，就可以使用  `systemctl` 命令来管理这个服务了。

### 例行性任务

at 命令

at 命令负责单次计划任务。

```shell
$ at now + 1min
> echo "hello"
> <EOT> （按下 Ctrl + D)
job 3 at Sat Apr 18 16:16:00 2020   # 任务编号与任务开始时间
```

crontab 命令

cron 命令负责周期性的任务设置，与 at 略有不同的是，cron 的配置大多通过配置文件实现。

```shell
# 分   时   日   月   星期  | 命令
# 下面是几个示例
*  *  *  *  *  echo "hello" >> ~/count
# 每分钟输出 hello 到家目录下 count 文件
0,15,30,45 0-6 * JAN SUN  command
# 随意举一个例子，翻译过来是每年一月份的每个星期日半夜 0 点到早晨 6 点每 15 分钟随便做点什么
# 反映了 crontab 中大部分语法。
5  3  *  *  * curl 'http://ip.42.pl/raw' | mail -s "ip today" xxx@xxx.com
# 每天凌晨 3 点 05 分将查询到的公网 ip 发送到自己的邮箱 （因为半夜 3 点重新拨号）
```

[该网站](https://crontab.guru/)可以将配置文件中的时间字段翻译为日常所能理解的时间表示。

## 参考资料：
{:.no_toc}

[1] [进程、前后台、服务与例行性任务](https://101.lug.ustc.edu.cn/Ch04/)

[2] [MIssing-semester：命令行环境](https://missing-semester-cn.github.io/2020/command-line/)
