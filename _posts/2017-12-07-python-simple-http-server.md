---
layout: post
title: python搭建简易的http服务器
categories: [python]
description: python搭建简易的http服务器
keywords: python, 服务器, http
---


<h2 align = "center">python搭建简易的http服务器</h2>

<br/>

两台机器都连在同一个无线路由器上，但是没有带U盘，想在两台电脑之间传文件怎么办？

windows系统实际上有很好的解决办法，右键共享文件即可，然后另一条windows电脑便可以远程访问。但是Linux就没有这样方便的功能了。

幸好电脑上装了python环境。用python创建一个http服务器，便可以实现这个简单的功能：

打开需要共享的文件夹目录，在命令行输入：

``` bash

python -m http.server 517

```

517是我设置的端口，在另一台机器的浏览器输入本机的 ip:517 就可以访问本机的文件。

<img src="http://blog-1255541504.file.myqcloud.com/_post/python-server-1.jpg" style="border:none;width: 400.0px;margin: center;"/>
