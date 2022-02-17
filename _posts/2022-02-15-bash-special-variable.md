---
layout: post
title: bash中常用的特殊变量
subtitle: 
tags: [Shell]
---

- `$0` - 脚本名
- `$1` 到 `$9` - 脚本的参数。 `$1` 是第一个参数，依此类推。
- `$@` - 所有参数
- `$#` - 参数个数
- `$?` - 前一个命令的返回值
- `$$` - 当前脚本的进程识别码
- `!!` - 完整的上一条命令，包括参数。常见应用：当你因为权限不足执行命令失败时，可以使用 `sudo !!`再尝试一次。
- `$_` - 上一条命令的最后一个参数。如果你正在使用的是交互式shell，你可以通过按下 `Esc` 之后键入 . 来获取这个值。

> [shebang](https://en.wikipedia.org/wiki/Shebang_(Unix)) - 在shebang中使用env命令可以提高脚本的可移植性。



test.sh:

```shell
#!/usr/bin/env bash

echo "Starting program at $(date)" # Date will be substituted

echo "Running program $0 with $# arguments with pid $$"

for file in "$@"; do
    grep foobar "$file" > /dev/null 2> /dev/null
    # When pattern is not found, grep has exit status 1
    # We redirect STDOUT and STDERR to a null register since we do not care about them
    if [[ $? -ne 0 ]]; then
        echo "File $file does not have any foobar, adding one"
        echo "# foobar" >> "$file"
    fi
done
```



##### 参考资料：

[1] [Advanced Bash-Scripting Guide](https://tldp.org/LDP/abs/html/special-chars.html)

[2] [MIssing-semester：Shell 工具和脚本](https://missing-semester-cn.github.io/2020/shell-tools/)