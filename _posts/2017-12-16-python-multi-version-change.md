---
layout: post
title: Linux多个Python版本切换，设置默认python
categories: [python, Linux]
description: Linux多个Python版本切换，设置默认python
keywords: python, Linux
---
update-alternatives是Debian提供的一个工具(非Debian系的就不用看了)，原理类似于上面一个办法，也是通过链接的方式，但是其切换的过程非常方便。

首先看一下update-alternatives的帮助信息：

$ update-alternatives --help
用法：update-alternatives [<选项> ...] <命令>

命令：
  --install <链接> <名称> <路径> <优先级>
    [--slave <链接> <名称> <路径>] ...
                           在系统中加入一组候选项。
  --remove <名称> <路径>   从 <名称> 替换组中去除 <路径> 项。
  --remove-all <名称>      从替换系统中删除 <名称> 替换组。
  --auto <名称>            将 <名称> 的主链接切换到自动模式。
  --display <名称>         显示关于 <名称> 替换组的信息。
  --query <名称>           机器可读版的 --display <名称>.
  --list <名称>            列出 <名称> 替换组中所有的可用候选项。
  --get-selections         列出主要候选项名称以及它们的状态。
  --set-selections         从标准输入中读入候选项的状态。
  --config <名称>          列出 <名称> 替换组中的可选项，并就使用其中
                           哪一个，征询用户的意见。
  --set <名称> <路径>      将 <路径> 设置为 <名称> 的候选项。
  --all                    对所有可选项一一调用 --config 命令。

<链接> 是指向 /etc/alternatives/<名称> 的符号链接。
    (如 /usr/bin/pager)
<名称> 是该链接替换组的主控名。
    (如 pager)
<路径> 是候选项目标文件的位置。
    (如 /usr/bin/less)
<优先级> 是一个整数，在自动模式下，这个数字越高的选项，其优先级也就越高。

选项：
  --altdir <目录>          改变候选项目录。
  --admindir <目录>        设置 statoverride 文件的目录。
  --log <文件>             改变日志文件。
  --force                  就算没有通过自检，也强制执行操作。
  --skip-auto              在自动模式中跳过设置正确候选项的提示
                           (只与 --config 有关)
  --verbose                启用详细输出。
  --quiet                  安静模式，输出尽可能少的信息。不显示输出信息。
  --help                   显示本帮助信息。
  --version                显示版本信息。
我们仅需要了解3个参数就行了

--install <链接> <名称> <路径> <优先级> ：建立一组候选项
--config <名称> ：配置 <名称>组中的可选项，并选择使用其中哪一个
--remove <名称> <路径> ：从 <名称> 中去掉 <路径>选项
首先我们先看一下有没有关于Python的可选项：

$ update-alternatives --display python
update-alternatives: 错误: 无 python 的候选项
那首先先建立python的组,并添加Python2和Python3的可选项

$ sudo update-alternatives --install /usr/bin/python python /usr/bin/python2.7 2 # 添加Python2可选项，优先级为2
$ sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.4 1 #添加Python3可选项，优先级为1
注意，这里的 /usr/bin/python 链接文件，两个可选项必须是一样的，这样这个链接文件才可以选择两个不同的可选项去链接。

这时如果我们查看 /usr/bin/python 这个文件时，会发现它已经链接到了 /etc/alternatives/python 。

lrwxrwxrwx 1 root root        24  6月 19 18:39 python -> /etc/alternatives/python
然后我们再看一下版本

$ python --version
Python 2.7.6
为什么还是Python2，看一下配置

$ sudo update-alternatives --config python
有 2 个候选项可用于替换 python (提供 /usr/bin/python)。

  选择       路径              优先级  状态
------------------------------------------------------------
* 0            /usr/bin/python2.7   2         自动模式
  1            /usr/bin/python2.7   2         手动模式
  2            /usr/bin/python3.4   1         手动模式
要维持当前值[*]请按回车键，或者键入选择的编号：
原来是因为默认选中了自动模式，而Python2的优先级高于Python3，这时候只要键入2，就可以使用Python3了。

如果你想要删除某个可选项的话：

$ sudo update-alternatives --remove python /usr/bin/python2.7
