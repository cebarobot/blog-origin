---
title: "为智龙 1C 开发板安装工具链"
tags: []
date: 2020-10-18 11:40:48
categories:
thumbnail:
---

在参加比赛，需要智龙 1C 开发板。

环境如下：
* WSL 2, Ubuntu

## 安装交叉编译工具 `gcc-4.3-ls232`

* 从 [龙芯开源社区](http://www.loongnix.org/index.php/Cross-compile) 下载交叉编译工具 `gcc-4.3-ls232`：
  ```shell
  $ wget http://ftp.loongnix.org/toolchain/gcc/release/gcc-4.3-ls232.tar.gz
  ```
* 解压到 `/opt` 目录：
  ```shell
  $ tar zxvf gcc-4.3-ls232.tar.gz -C /opt
  ```
* 添加环境变量：
  * 在 `~/.bashrc` 文件末尾，添加：
    ```bash
    export PATH=/opt/gcc-4.3-ls232/bin:$PATH
    ```
  * 重新加载 `~/.bashrc`：
    ```shell
    $ source ~/.bashrc
    ```
* **重要** 安装 `lib32ncurses6` 库，以使 32 位的 `gcc-4.3-ls232` 正常运行：
  ```shell
  $ sudo apt install lib32ncurses6
  ```
  > 这个地方教程上提示安装 `lsb-core`，但似乎安装上面的这个库就可以了。
* 检查 `gcc-4.3-ls232` 版本信息，以确认安装成功：
  ```shell
  $ mipsel-linux-gcc -v
  ```