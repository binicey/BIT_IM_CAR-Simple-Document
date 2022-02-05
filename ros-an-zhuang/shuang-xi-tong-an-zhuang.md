# 双系统安装

**Author：**Iccccy     **Data：**2021-10-19

## 准备材料

### 镜像下载

推荐国内开源镜像站：

+ [阿里巴巴开源镜像站-OPSX镜像站-阿里云开发者社区 (aliyun.com)](https://developer.aliyun.com/mirror/)

注意事项：

+ 推荐选择OS镜像Ubuntu18.04LTS下载与安装；
+ 本文档的绝大部分实验是基于Ubuntu18.04LTS环境下实现；
+ 新手建议：下载时注意选择desktop版本，即为桌面完整版本；

### U盘准备

储存容量：≥16GB



### 镜像刻录软件

推荐使用**UltraISO**软件，进行系统镜像刻录。

本文档暂不提供刻录软件下载。

## 安装过程

安装过程参考可参照以下视频内容：

[【ubuntu20.04】10分钟win10安装ubuntu20.04双系统（无需Bios设置）_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV11k4y1k7Li?from=search&seid=12150875958768971485&spm_id_from=333.337.0.0)

❗**在安装之前，请仔细阅读以下内容，并细心操作**❗

注意事项：

+ 任何提示格式化和分区命令的操作，请仔细思考并按照要求进行；
+ 对于重要的数据，请在进行不知名操作前进行备份；
+ 如不清楚自己进行的操作，请求助于专业人士或者搜索引擎，勿盲目操作；
+ 数据无价。

## Ubuntu初始配置

### 软件源更换

#### 图形方式

+ 在**设置-软件与更新**中，选择中国国内镜像；

+ 在 **终端** 中运行

  ```shell
  sudo apt update
  ```

#### 编辑方式

+ 设置软件源

  + 清华源官网：[ubuntu | 镜像站使用帮助 | 清华大学开源软件镜像站 | Tsinghua Open Source Mirror](https://mirror.tuna.tsinghua.edu.cn/help/ubuntu/)

+ 编辑软件源列表

  **运行如下命令：**

  ```shell
  sudo gedit /etc/apt/sources.list	
  ```

  **在打开的编辑器中，文档末尾添加以下内容：**

  ```shell
  # 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
  deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
  # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
  deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
  # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
  deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
  # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
  deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse
  # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse
  
  # 预发布软件源，不建议启用
  # deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
  # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
  ```

  **在终端中运行：**

  ```shell
  sudo apt update
  ```

### 推荐软件下载

#### Terminator

多窗口终端编辑器；

```shell
sudo apt-get install terminator
```

#### VScode

实用的文档编辑器；

> [Visual Studio Code - Code Editing. Redefined](https://code.visualstudio.com/)

#### Zshell&~Oh my zsh

强大的shell工具；

> [ZSH - THE Z SHELL (sourceforge.io)](https://zsh.sourceforge.io/)
>
> [Oh My Zsh - a delightful & open source framework for Zsh](https://ohmyz.sh/)

