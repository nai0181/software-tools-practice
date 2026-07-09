---
title: 实验七 SSH远程管理与文件传输
categories:
  - 软件开发工具实践
date: 2026-07-09 16:43:58
tags:
---

## 一、实验目标

本实验主要学习 SSH 远程登录、SSH 密钥认证、scp 文件传输以及 rsync 目录同步。通过本实验，掌握在 Windows 宿主机与 Ubuntu 虚拟机之间，以及多台 Ubuntu 虚拟机之间进行远程管理和文件传输的方法。

<!-- more -->

## 二、实验环境

- 宿主机系统：Windows 11
- 虚拟机系统：Ubuntu Server 22.04 LTS
- 虚拟机软件：Oracle VirtualBox
- 实验节点：
  - node1：192.168.56.102
  - node2：192.168.56.103
  - node3：192.168.56.104
- 登录用户：dev
- 远程管理工具：OpenSSH、scp、rsync

## 三、SSH 远程登录

SSH 是一种安全的远程登录协议，可以让用户从一台计算机远程登录另一台计算机并执行命令。

在 Windows PowerShell 中，可以通过以下命令登录 node1：

    ssh dev@192.168.56.102

其中：

- ssh 表示使用 SSH 协议
- dev 表示登录的用户名
- 192.168.56.102 表示 node1 的 IP 地址

登录成功后，可以执行：

    hostname
    whoami

验证当前主机名和登录用户。

## 四、SSH 密钥登录

普通 SSH 登录需要输入密码。为了提高安全性和便利性，可以使用 SSH Key 进行密钥登录。

SSH 密钥由两部分组成：

    私钥：保存在本机，不能泄露
    公钥：放到服务器上，用于验证身份

### 1. 在 Windows 上生成 SSH Key

在 Windows PowerShell 中执行：

    ssh-keygen -t ed25519 -f "$env:USERPROFILE\.ssh\vm_lab_ed25519" -C "software-tools-practice-vm"

生成两个文件：

    vm_lab_ed25519      私钥
    vm_lab_ed25519.pub  公钥

### 2. 将公钥添加到 node1

执行：

    type "$env:USERPROFILE\.ssh\vm_lab_ed25519.pub" | ssh dev@192.168.56.102 "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys"

该命令将 Windows 上的公钥追加写入 node1 的：

    ~/.ssh/authorized_keys

authorized_keys 文件用于保存允许登录该服务器的公钥。

### 3. 使用密钥登录 node1

执行：

    ssh -i "$env:USERPROFILE\.ssh\vm_lab_ed25519" dev@192.168.56.102

如果不需要输入 Linux 用户密码即可登录，说明密钥登录配置成功。

## 五、配置 SSH 简写登录

为了简化 SSH 命令，可以编辑 Windows 本地 SSH 配置文件：

    notepad "$env:USERPROFILE\.ssh\config"

添加以下内容：

    Host node1
        HostName 192.168.56.102
        User dev
        IdentityFile ~/.ssh/vm_lab_ed25519

保存后，可以直接使用：

    ssh node1

登录 node1。

这说明 SSH config 可以为远程服务器配置别名，减少重复输入复杂命令。

## 六、scp 文件传输

scp 是基于 SSH 的远程文件复制工具，可以在本地和远程服务器之间传输文件。

### 1. Windows 上传文件到 node1

在 Windows 桌面创建测试文件：

    echo "Hello from Windows" > "$HOME\Desktop\win-test.txt"

上传到 node1：

    scp "$HOME\Desktop\win-test.txt" node1:~/win-test.txt

登录 node1 验证：

    ssh node1
    ls -l ~/win-test.txt
    cat ~/win-test.txt

如果可以看到文件内容，说明 Windows 到 node1 的文件上传成功。

### 2. node1 下载文件到 Windows

在 node1 中创建文件：

    echo "Hello from node1" > ~/linux-test.txt

退出回 Windows 后执行：

    scp node1:~/linux-test.txt "$HOME\Desktop\linux-test.txt"

在 Windows 中查看：

    type "$HOME\Desktop\linux-test.txt"

如果可以看到文件内容，说明 node1 到 Windows 的文件下载成功。

### 3. 传输整个文件夹

scp 默认传输单个文件。如果要传输文件夹，需要使用 `-r` 参数。

在 Windows 中创建测试文件夹：

    mkdir "$HOME\Desktop\scp-folder"
    echo "file one" > "$HOME\Desktop\scp-folder\a.txt"
    echo "file two" > "$HOME\Desktop\scp-folder\b.txt"

上传整个文件夹：

    scp -r "$HOME\Desktop\scp-folder" node1:~/scp-folder

登录 node1 验证：

    find ~/scp-folder -type f
    cat ~/scp-folder/a.txt
    cat ~/scp-folder/b.txt

其中，`-r` 表示递归复制，即复制文件夹及其内部所有内容。

## 七、虚拟机之间传输文件

在多机环境中，也可以通过 scp 在不同 Ubuntu 虚拟机之间传输文件。

### 1. node1 创建测试文件

在 node1 中执行：

    echo "Hello from node1 to node2" > ~/node1-to-node2.txt
    echo "Hello from node1 to node3" > ~/node1-to-node3.txt

### 2. node1 传输到 node2

执行：

    scp ~/node1-to-node2.txt dev@node2:~/from-node1.txt

### 3. node1 传输到 node3

执行：

    scp ~/node1-to-node3.txt dev@node3:~/from-node1.txt

### 4. 验证 node2 和 node3 文件

在 node1 中远程查看 node2：

    ssh dev@node2 "hostname && cat ~/from-node1.txt"

远程查看 node3：

    ssh dev@node3 "hostname && cat ~/from-node1.txt"

如果能够看到对应主机名和文件内容，说明虚拟机之间的文件传输成功。

## 八、rsync 目录同步

rsync 是一种更适合目录同步的工具。与 scp 相比，rsync 可以只同步发生变化的文件，效率更高。

### 1. 检查 rsync 是否安装

在 node1 中执行：

    rsync --version

检查 node2：

    ssh dev@node2 "rsync --version"

如果提示 command not found，可以安装：

    sudo apt update
    sudo apt install -y rsync

### 2. 创建同步目录

在 node1 中执行：

    mkdir -p ~/sync-demo
    echo "version 1" > ~/sync-demo/a.txt
    echo "hello rsync" > ~/sync-demo/b.txt

### 3. 同步到 node2

执行：

    rsync -av ~/sync-demo/ dev@node2:~/sync-demo/

其中：

- -a 表示 archive，归档模式，保留文件属性并递归同步目录
- -v 表示 verbose，显示同步过程
- 源目录最后的 `/` 表示同步目录中的内容

### 4. 验证同步结果

执行：

    ssh dev@node2 "find ~/sync-demo -type f -maxdepth 1 -print && cat ~/sync-demo/a.txt && cat ~/sync-demo/b.txt"

如果 node2 上出现 a.txt 和 b.txt，说明同步成功。

### 5. 修改文件后再次同步

在 node1 中修改文件并新增文件：

    echo "version 2" > ~/sync-demo/a.txt
    echo "new file" > ~/sync-demo/c.txt

再次同步：

    rsync -av ~/sync-demo/ dev@node2:~/sync-demo/

验证：

    ssh dev@node2 "cat ~/sync-demo/a.txt && cat ~/sync-demo/c.txt"

可以看到 node2 上的文件已更新。

### 6. 使用 --delete 保持目录一致

默认情况下，rsync 会新增和更新文件，但不会删除目标端多出来的文件。

先在 node1 中删除 b.txt：

    rm ~/sync-demo/b.txt

执行普通同步：

    rsync -av ~/sync-demo/ dev@node2:~/sync-demo/

此时 node2 上的 b.txt 可能仍然存在。

如果需要让目标目录和源目录完全一致，可以使用：

    rsync -av --delete ~/sync-demo/ dev@node2:~/sync-demo/

再次查看 node2：

    ssh dev@node2 "ls ~/sync-demo"

此时目标端多余的 b.txt 会被删除。

因此：

    scp 适合简单复制文件
    rsync 适合同步目录
    rsync --delete 可以让目标目录与源目录保持完全一致

## 九、实验结果

本实验完成了以下内容：

- 使用 SSH 登录 Ubuntu 虚拟机
- 生成实验专用 SSH Key
- 配置 node1 的密钥登录
- 使用 SSH config 实现 ssh node1 简写登录
- 使用 scp 完成 Windows 与 node1 之间的文件上传和下载
- 使用 scp 传输整个文件夹
- 使用 scp 完成 node1 到 node2、node3 的文件传输
- 使用 rsync 将 node1 的目录同步到 node2
- 理解 rsync 与 scp 的区别
- 理解 rsync --delete 的作用

## 十、实验总结

通过本实验，我掌握了 Linux 服务器远程管理和文件传输的基本方法。

SSH 用于远程登录服务器，SSH Key 可以实现更安全和方便的免密码登录，SSH config 可以简化连接命令。scp 适合进行简单文件复制，rsync 更适合目录同步和增量更新。

本实验将前面搭建的多台 Ubuntu 虚拟机真正连接起来，使 node1、node2、node3 不仅能够互相 ping 通，还能够通过 SSH 和 scp 进行远程操作和文件传输。这为后续 Web 服务部署和自动化同步打下了基础。