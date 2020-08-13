---
title: "利用 VirtualBox 和 VS Code 手动搭建开发环境"
date: 2019-05-27 14:01:13
categories: "开发者手册"
tags: ["Visual Studio Code", "VirtualBox", "Ubuntu", "开发环境"]
thumbnail:
---

> 此文章发布于 2019 年，其中描述的内容可能已经过时

好久没有写博客了……这次的标题稍微有些大，实际内容是搭建一个能舒服的写计算机科学导论课的编程作业的环境。我们学校的计算机科学导论比较奇葩，这个实验要求用 go 语言写一个把文本文档隐藏进一个图片的程序。表示我一致不太明白为什么要用go……

## 背景
这门课程的助教老师提供了一个 Ubuntu 16.04 的 VirtualBox 虚拟机镜像，里面配置好了 go 语言的环境。但由于虚拟化的问题，在虚拟机里面直接拿 Gedit 或者其他的编辑器非常卡顿，很不舒服。

于是尝试直接在 Windows 10 下用 VS Code 搭建 go 的开发环境，然而 VS Code 的 go 语言扩展需要一系列 go 的包，但由于众所周知的原因，尝试了各种方法总是不能从 Google 的服务器把这些包拖回来……遂放弃。

之后想到之前接触到的 Laravel 框架，它提供的开发环境就是一个虚拟机。于是就考虑能不能用类似的思路搞这个 go 的开发环境呢？

## 过程
基本上有这么几件事要做：安装增强工具、挂载源代码目录，映射端口，配置 ssh。其实都没有啥难度，但第一次搞就有些折腾……

### 安装增强工具
打开虚拟机，点击`设置/安装增强功能`，VirtualBox 会把安装包作为 CD 挂载到 Ubuntu 里面。终端打开 `/media/VBOXADDITIONS_<version>\`，执行命令
```bash
./VBoxLinuxAddtion.run
```

### 挂载源代码目录
首先，在 Windows 下建立储存源代码的文件夹，我一般图方便就放到用户文件夹下面了（毕竟 Git Bash 里面的 `~` 就是这个嘛）。

然后在 VirtualBox 的设置里面添加共享文件夹，如图：

![添加共享文件夹](p1.png)

* 选择共享文件夹路径（目录里面不要加中文）
* 填写共享文件夹名称，这里是`go`
* 勾选自动挂载
* 挂载点不需要管，没卵用

进入虚拟机，共享文件夹会挂载到 `/media/sf_<share_folder_name>`，这里我们是`/media/sf_go`。需要注意的是，这个文件夹的所属组是`vboxsf`，我们需要把当前用户添加到这个用户组里面：
```bash
sudo usermod -a -G vboxsf <usernanme>   # 一般的命令
sudo usermod -a -G vboxsf sai           # 这次实验中的命令
```

之后，我们需要把这个共享文件夹软连接到我们的工作目录：
```bash
ln -s /media/sf_<share_folder_name> <workspace_path>    # 通用的命令
ln -s /media/sf_go ~/workspace/go/src/project           # 这次实验中的命令
```

### 映射端口
这里有两种选择，使用 `NAT 网络` 或者 `网络地址转换（NAT）`。前者需要先建立 NAT 网络，其余大同小异。

在`全局设定/网络/编辑NAT网络`（`NAT 网络`）或者`虚拟机设置/网络/高级`里面的`端口转发`中设置需要转发的端口，其中`网络地址转换（NAT）`情况下不需要设置`子系统IP`。这里我们只需要把 ssh 的 22 端口映射出来（如映射到 2322）：

![端口转发](p2.png)

### 配置 ssh
桌面版的 Ubuntu 可能需要手动安装 ssh 服务器：
```bash
sudo apt install openssh-server     # 安装 openssh-server
sudo service ssh start              # 启动 ssh 服务器
```

我们需要把宿主机的 ssh 密钥复制到虚拟机中，在 Git bash 或者 WSL terminal 中运行：
```bash
ssh-keygen -t rsa -C "youremail@example.com"    # 生成 ssh 密钥（已有的请忽略）
ssh-copy-id <username>@<host> -p <port>         # 复制 ssh 密钥到远程计算机，“-p <port>”可以省略
ssh-copy-id sai@127.0.0.1 -p 2322               # 这次实验中的命令，-p 表示端口号
```

之后尝试用 ssh 登陆远程计算机：
```bash
ssh <username>@<host> -p <port>         # 通用的命令
ssh sai@127.0.0.1 -p 2322               # 这次实验中的命令
```

如果出现下面的情况，说明成功：

![ssh 登陆成功](p3.png)

至此，我们对虚拟机的配置就全部完成了。

## 后记
完成配置之后，无界面启动虚拟机，用 VS Code 打开代码文件夹和终端，异常舒服～

![完成！](p4.png)

我曾在之前尝试用 zsh 美化终端，但可能由于 Git bash 的锅，总是有 Unicode 字符显示不出来……卸载 zsh 之后又发现 ssh 无法登陆，原因是安装 zsh 的时候把`/etc/passwd`里面的默认终端改掉了。

## 参考资料
- [ubuntu在virtualbox和vmware共享文件夹的问题关键点](https://www.jianshu.com/p/52c4ae8e512f)
- [VirtualBox中linux虚拟机和主机间的共享文件设置](https://www.cnblogs.com/lilyonly/p/6518749.html)
- [利用ssh-copy-id复制公钥到多台服务器](https://www.cnblogs.com/panchong/p/6027138.html)
