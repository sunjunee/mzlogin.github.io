---
layout: post
title: Windows下tensorflow gpu版的离线安装(cuda 8.0 + cudnn 5.1 + tf 1.2)
categories: [深度学习]
description: Windows下tensorflow gpu版的离线安装(cuda 8.0 + cudnn 5.1 + tf 1.2)
keywords: 机器学习, 深度学习, tensorflow, gpu
---

<h2 align = "center">Windows下tensorflow gpu版的离线安装(cuda 8.0 + cudnn 5.1 + tf 1.2)</h2>

开发用的电脑，因为各种原因不能连网，最近电脑加了显卡，试着装了下GPU版的tensorflow，中间遇到了一些坑，这里总结一下。

下面具体介绍一下Windows下tensorflow gpu版的离线安装过程。

### 1. 系统及软件版本

* 系统：windows 7 64 bits
* 显卡: Nvidia k4200
* Python: 3.5.2 (anaconda3 4.2.0)
* Cuda: 8.0
* cudnn: 5.1
* tensorflow: 1.2.0 gpu版

### 2. CUDA和CUDNN及vs2015安装

#### 2.1 Cuda
首先安装CUDA，这里要注意，安装8.0或8.0以下的的cuda版本，因为tensorflow目前只支持到cuda8.0。如果直接搜索进入nvidia官网的话，官网给的链接是9.0的，选择版本的位置也比较隐蔽，因此直接从下面这个链接下载吧：

<a href = "https://developer.nvidia.com/cuda-80-ga2-download-archive">cuda_8.0.61_windows.exe</a>

我的系统是win7 64位，所以安装系统版本选，下载完之后，傻瓜式安装就行。

#### 2.2 Cudnn

然后需要去安装cudnn的库，同样也是去官网，但是需要注册一个nvidia的开发者账号，然后就可以下载了，下载链接在下面，注意系统版本和cuda版本，这里用的是5.1：

<a href = "https://developer.nvidia.com/cudnn">cudnn-8.0-windows7-x64-v5.1.zip</a>

下载之后，不需要安装，而是将下载的文件放到cuda的安装目录下。打开cudnn压缩包，里面应该有三个文件夹，里面有相应的文件，我们需要手工的将这三个文件夹里的文件，复制到cuda的安装目录下，每个文件放置的位置，和cudnn给的位置要一致。

#### 2.3 VS2015

直接下载安装即可：

<a href="https://www.microsoft.com/en-us/download/details.aspx?id=53587">VS 2015</a>

### 3. Python及tensorflow依赖包安装

为了方便安装，这里直接安装Anaconda，具体安装的版本是Anaconda3 4.2.0，官网可能找不到下载链接，但是可以通过下面镜像来下载安装：

<a href="http://mirrors.ustc.edu.cn/anaconda/archive/Anaconda3-4.2.0-Windows-x86_64.exe">Anaconda3-4.2.0-Windows-x86_64.exe</a>

然后需要安装tensorflow gpu版的依赖环境，除了anaconda带了的python包，还有一些需要手动安装的，版本和下载链接都在下面，挨个下载，然后用pip安装即可：

| 依赖 | 使用版本 | 下载 |
| - | :-: | -: |
| protobuf >= 3.2.0|protobuf-3.2.0.tar.gz | <a href = "https://mirrors.ustc.edu.cn/pypi/web/simple/protobuf/">下载</a> |
| werkzeug >= 0.11.10|Werkzeug-0.11.11.tar.gz | <a href = "https://mirrors.ustc.edu.cn/pypi/web/simple/werkzeug/">下载</a> |
| html5lib = 0.9999999|html5lib-0.9999999.tar.gz | <a href = "https://mirrors.ustc.edu.cn/pypi/web/simple/html5lib/">下载</a> |
| setuptools_scm = 1.2.0|setuptools_scm-1.2.0-py2.py3-none-any.whl | <a href = "https://mirrors.ustc.edu.cn/pypi/web/simple/setuptools-scm/">下载</a> |
| backports.weakref == 1.0rc1 | 1.0rc1 | <a href = "https://github.com/pjdelport/backports.weakref/archive/v1.0rc1.tar.gz">下载</a> |
| bleach == 1.5.0|bleach-1.5.0.tar.gz | <a href = "https://mirrors.ustc.edu.cn/pypi/web/simple/bleach/">下载</a> |
| Markdown|Markdown-2.2.0.tar.gz | <a href = "https://mirrors.ustc.edu.cn/pypi/web/simple/markdown/">下载</a> |
### 4. tensorflow安装

下载tensorflow 1.2.0 GPU版本:

<a href = "https://mirrors.ustc.edu.cn/pypi/web/packages/8f/ea/59719f0d362c44fd15ac7b131faaf980e80e259beb03d669fe962df1577a/tensorflow_gpu-1.2.0-cp35-cp35m-win_amd64.whl#md5=9835b4060efe44b332cf0098602fd0ca">tensorflow_gpu-1.2.0-cp35-cp35m-win_amd64.whl</a>

然后用pip安装即可。

### 5. 福利

所有的资源，都打包上传到百度云了，如果嫌挨个下载安装麻烦，可以打包下载：

<a href = "http://pan.baidu.com/s/1o82zQAY">cuda + cudnn + vs 下载</a>

<a href = "http://pan.baidu.com/s/1hr815Ne">anaconda + tensorflow + 依赖包</a>
