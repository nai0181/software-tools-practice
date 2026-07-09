---
title: 实验六 Linux环境配置
categories:
  - 软件开发工具实践
date: 2026-07-09 15:49:51
tags:
---

## 一、实验目标

本实验主要学习 Linux 服务器环境中的基础命令、文件目录操作、权限管理、sudo 管理员权限以及环境变量 PATH 的配置方法。

通过本实验，掌握在 Ubuntu Server 中进行基本操作的能力，为后续远程管理和网站部署打下基础。

<!-- more -->

## 二、实验环境

- 操作系统：Ubuntu Server 22.04 LTS
- 虚拟机软件：Oracle VirtualBox
- 实验主机：node1
- 主机 IP：192.168.56.102
- 登录用户：dev
- 远程连接方式：SSH

## 三、Linux 文件系统与基础命令

Linux 采用统一的目录树结构，所有文件和目录都从根目录 `/` 开始。

常见目录包括：

    /home    普通用户家目录
    /etc     系统配置文件目录
    /usr     系统程序和资源目录
    /var     日志、缓存等可变数据目录
    /tmp     临时文件目录

本实验中，dev 用户的家目录为：

    /home/dev

可以使用 `~` 表示当前用户的家目录。

### 1. 查看当前状态

登录 node1 后，执行以下命令：

    hostname
    whoami
    pwd

其中：

- hostname 用于查看当前主机名
- whoami 用于查看当前登录用户
- pwd 用于查看当前所在目录

### 2. 创建实验目录

执行以下命令创建实验目录：

    cd ~
    mkdir -p linux-lab/docs
    mkdir -p linux-lab/backup
    mkdir -p linux-lab/scripts
    cd linux-lab
    ls -la

其中，`mkdir -p` 可以在父目录不存在时一并创建。

### 3. 创建和查看文件

使用 echo 和重定向创建文件：

    echo "Hello Linux" > docs/readme.txt

查看文件内容：

    cat docs/readme.txt

其中，`>` 表示将命令输出写入文件。

### 4. 复制、移动和重命名文件

复制文件：

    cp docs/readme.txt backup/readme-copy.txt

重命名文件：

    mv backup/readme-copy.txt backup/readme-v1.txt

Linux 中，`mv` 既可以移动文件，也可以重命名文件。

### 5. 查找文件和内容

查找文件内容：

    grep "Linux" docs/readme.txt

查找文件：

    find . -maxdepth 2 -type f

其中，`.` 表示当前目录。

## 四、Linux 权限管理

Linux 文件权限主要由三类用户和三种权限组成。

三类用户包括：

    owner   文件所有者
    group   文件所属组
    others  其他用户

三种权限包括：

    r   read，读取权限
    w   write，写入权限
    x   execute，执行权限

查看文件权限：

    ls -l docs/readme.txt

示例输出：

    -rw-rw-r-- 1 dev dev 12 Jul 9 13:00 docs/readme.txt

其中，`-rw-rw-r--` 表示：

- 文件所有者可以读写
- 同组用户可以读写
- 其他用户只能读取

### 1. 创建脚本文件

创建一个简单脚本：

    echo 'echo "Hello from script"' > scripts/hello.sh

查看权限：

    ls -l scripts/hello.sh

此时文件通常没有执行权限。

### 2. 尝试运行脚本

执行：

    ./scripts/hello.sh

如果没有执行权限，会出现：

    Permission denied

这说明文件内容虽然正确，但当前没有执行权限。

### 3. 添加执行权限

执行：

    chmod u+x scripts/hello.sh

其中：

- chmod 表示修改权限
- u 表示文件所有者
- +x 表示增加执行权限

再次运行：

    ./scripts/hello.sh

输出：

    Hello from script

通过该实验可以理解：Linux 中脚本文件不仅需要有正确内容，还需要具备执行权限。

## 五、sudo 与 root 权限

Linux 中最高权限用户是 root。普通用户 dev 只能修改自己有权限的文件，不能随意修改系统配置文件。

例如，查看系统 hosts 文件：

    cat /etc/hosts

普通用户可以读取该文件。

但如果直接编辑：

    nano /etc/hosts

保存时会因为权限不足而失败。

使用 sudo 可以临时以管理员权限执行命令：

    sudo nano /etc/hosts

sudo 的含义是：以管理员权限执行后面的命令。

查看 hosts 文件权限：

    ls -l /etc/hosts

示例：

    -rw-r--r-- 1 root root ... /etc/hosts

这表示该文件属于 root 用户，普通用户只有读取权限，没有写入权限。

通过该实验可以理解：

    sudo = 临时使用 root 权限执行命令

在修改系统配置、安装软件、管理服务时通常需要使用 sudo。

## 六、环境变量 PATH

Linux 中很多命令本质上也是程序文件，例如 ls、cat、mkdir 等。

查看命令位置：

    which ls
    which cat
    which mkdir

示例输出：

    /usr/bin/ls

这说明 ls 实际上是位于 `/usr/bin/ls` 的程序。

### 1. 查看 PATH

执行：

    echo $PATH

PATH 是系统查找命令的目录列表。当用户输入命令时，系统会依次到 PATH 中记录的目录里查找对应程序。

### 2. 创建自己的命令

创建一个自定义脚本：

    echo 'echo "This is my own command."' > scripts/mycmd
    chmod u+x scripts/mycmd

直接通过路径运行：

    ./scripts/mycmd

输出：

    This is my own command.

如果直接输入：

    mycmd

一开始会提示 command not found。

原因是 `mycmd` 所在的目录没有加入 PATH，系统不知道去哪里查找该命令。

### 3. 临时修改 PATH

执行：

    export PATH="$HOME/linux-lab/scripts:$PATH"

然后再次运行：

    mycmd

此时可以成功输出：

    This is my own command.

这说明将脚本所在目录加入 PATH 后，系统可以直接识别该命令。

## 七、配置 .bashrc 使 PATH 永久生效

临时 export 只在当前终端有效。重新登录后，配置可能失效。

为了让 PATH 永久生效，需要将配置写入 `~/.bashrc` 文件。

编辑 `.bashrc`：

    nano ~/.bashrc

在文件末尾添加：

    # Add my Linux lab scripts to PATH
    export PATH="$HOME/linux-lab/scripts:$PATH"

保存后执行：

    source ~/.bashrc

其中，source 用于立即重新加载配置文件。

退出 SSH 后重新登录：

    ssh dev@192.168.56.102

直接执行：

    mycmd

如果仍然可以输出：

    This is my own command.

说明 PATH 永久配置成功。

## 八、实验结果

本实验完成了 Linux 环境配置相关基础操作，包括：

- 使用 SSH 登录 Ubuntu Server
- 掌握 pwd、ls、cd、mkdir、cat、cp、mv、grep、find 等基础命令
- 创建实验目录和文件
- 理解 Linux 文件权限 rwx
- 使用 chmod 修改脚本执行权限
- 理解 sudo 和 root 权限
- 查看命令所在位置
- 理解 PATH 环境变量
- 将自定义脚本目录写入 .bashrc，使其永久生效

## 九、实验总结

通过本实验，我对 Linux 服务器环境有了更清晰的认识。

Linux 并不是简单地记忆命令，而是通过命令操作文件系统、管理权限和配置运行环境。基础命令对应文件和目录操作，权限系统控制用户能否读取、修改和执行文件，sudo 用于临时获取管理员权限，PATH 则决定系统如何查找命令。

本实验为后续 SSH 远程管理、文件传输和 Web 服务部署奠定了基础。
