---
layout: post
title: unity开发注意事项
subtitle: 
tags: [unity]
---

# unity项目打包到webgl平台的注意事项

### 1，项目地址问题

​	unity项目地址和打包的文件夹地址不能含有中文。

### 2，字体问题

​	webgl不支持Arial字体，需自行导入.ttf格式或.otf格式字体。

### 3， 压缩格式设置

​	将项目打包成webgl格式后在浏览器中打开卡住，同时浏览器console里报错Uncaught ReferenceError: unityFramework is not defined。
​	这时将压缩格式 改为 已禁用, 再次发布即可。

# unity和3dmax里的单位问题

unity中：1个单位长度=1m

3dmax中：系统单位*物理单位=显示单位



>参考资料：
>
>https://my.oschina.net/u/4309066/blog/3411315
>
>https://blog.csdn.net/m0_37921148/article/details/79848675
>
>http://blog.dou.li/unity3d-import-3dmax-model-scale.html
>
>https://blog.csdn.net/aikb6223/article/details/102349997