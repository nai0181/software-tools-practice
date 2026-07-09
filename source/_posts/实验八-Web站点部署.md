---
title: 实验八 Web站点部署
categories:
  - 软件开发工具实践
date: 2026-07-09 20:40:33
tags:
---

## 一、实验目标

本实验主要完成 Web 站点部署，将前面使用 Hexo 构建的课程展示网站部署到 Ubuntu Server 虚拟机中，并通过 Nginx 对外提供访问。

通过本实验，理解 Hexo 静态网站生成、服务器目录配置、Nginx Web 服务配置、文件同步部署等完整流程。

<!-- more -->

## 二、实验环境

- 宿主机系统：Windows 11
- 虚拟机系统：Ubuntu Server 22.04 LTS
- 部署服务器：node1
- node1 IP：192.168.56.102
- Web 服务器：Nginx
- 静态网站生成器：Hexo
- 远程连接方式：SSH
- 文件传输方式：scp / rsync

## 三、核心概念

### 1. Hexo

Hexo 是静态网站生成器。它可以将 Markdown 文章转换成 HTML、CSS、JS 等静态网页文件。

在本项目中，实验文章存放在：

    source/_posts

执行生成命令后，Hexo 会将网站成品输出到：

    public

### 2. Nginx

Nginx 是常见的 Web 服务器软件。它负责接收浏览器请求，并将服务器上的网页文件返回给浏览器。

在本实验中，Hexo 负责生成网页，Nginx 负责发布网页。

### 3. 静态网站部署

静态网站部署的基本流程是：

    编写 Markdown 文章
    使用 Hexo 生成 public 静态文件
    将 public 文件同步到服务器网站目录
    使用 Nginx 提供网页访问

## 四、安装并启动 Nginx

首先 SSH 登录 node1：

    ssh node1

安装 Nginx：

    sudo apt update
    sudo apt install -y nginx

检查 Nginx 服务状态：

    systemctl status nginx

如果看到：

    active (running)

说明 Nginx 已经正常运行。

在 node1 中测试：

    curl http://localhost

如果返回 Welcome to nginx 的 HTML 内容，说明 Nginx 本机访问正常。

在 Windows 浏览器中访问：

    http://192.168.56.102

如果看到 Nginx 默认欢迎页面，说明 Windows 可以通过浏览器访问 node1 上的 Web 服务。

## 五、创建课程网站部署目录

默认 Nginx 网站目录为：

    /var/www/html

为了单独管理课程网站，本实验创建新的部署目录：

    /var/www/course-site

执行：

    sudo mkdir -p /var/www/course-site
    sudo chown -R dev:dev /var/www/course-site

其中，chown 用于将该目录的所有者改为 dev 用户，方便后续通过 scp 或 rsync 上传网站文件。

由于本项目 GitHub Pages 的访问路径为：

    /software-tools-practice/

因此在服务器上创建对应子目录：

    mkdir -p /var/www/course-site/software-tools-practice

最终网站部署目录为：

    /var/www/course-site/software-tools-practice

## 六、配置 Nginx 站点

创建 Nginx 配置文件：

    sudo nano /etc/nginx/sites-available/course-site

配置内容如下：

    server {
        listen 80 default_server;
        server_name 192.168.56.102 localhost;

        root /var/www/course-site;
        index index.html;

        location / {
            try_files $uri $uri/ =404;
        }
    }

其中：

- listen 80 表示监听 HTTP 默认端口
- server_name 表示服务器名称
- root 表示网站根目录
- index 表示默认首页文件
- try_files 表示根据访问路径查找对应文件，找不到则返回 404

启用站点配置：

    sudo ln -s /etc/nginx/sites-available/course-site /etc/nginx/sites-enabled/course-site

删除默认站点：

    sudo rm -f /etc/nginx/sites-enabled/default

测试配置：

    sudo nginx -t

如果输出：

    syntax is ok
    test is successful

说明配置没有语法错误。

重新加载 Nginx：

    sudo systemctl reload nginx

## 七、生成 Hexo 静态网站

回到 Windows 本地 Hexo 项目目录：

    cd "$HOME\Documents\SoftwareDevToolsPractice\course-site"

执行：

    hexo clean
    hexo generate

其中：

- hexo clean 用于清理旧的生成文件和缓存
- hexo generate 用于生成静态网站文件

生成完成后，网站成品位于：

    public

## 八、部署 Hexo 网站到 node1

将 public 目录中的文件上传到 node1：

    scp -r .\public\* node1:/var/www/course-site/software-tools-practice/

也可以使用 rsync 同步：

    rsync -av --delete .\public\ node1:/var/www/course-site/software-tools-practice/

其中：

- scp 适合简单复制文件
- rsync 适合目录同步
- --delete 可以让服务器目录和本地 public 目录保持一致

部署完成后，访问：

    http://192.168.56.102/software-tools-practice/

如果可以看到 Hexo 课程展示网站，说明部署成功。

## 九、问题与解决方法

### 1. 浏览器无法访问 192.168.56.102

一开始浏览器无法访问 node1 的网站，后来发现是因为代理软件接管了局域网地址访问。

192.168.56.102 属于 VirtualBox Host-Only 内网地址，应该直接通过本机网络访问，而不应该走代理。

解决方法是关闭代理，或者将 192.168.56.0/24 加入代理绕过规则。

### 2. 访问网站出现 403 Forbidden

访问：

    http://192.168.56.102/software-tools-practice/

时出现 403 Forbidden。

原因是 Nginx 找到了目录，但目录中没有可访问的 index.html，或者目录权限不正确。

解决方法是确认 Hexo 的 public 内容正确上传到：

    /var/www/course-site/software-tools-practice/

并确保目录中存在：

    index.html

### 3. 页面能打开但没有样式

网站首页可以打开，但页面只有文字，没有 CSS 样式。

原因是 CSS、JS 等静态资源路径或目录权限存在问题。

通过检查发现，css 目录存在，但 Nginx 访问 CSS 文件时返回 404。

最终修改 Nginx 配置，将网站根目录设置为：

    root /var/www/course-site;

同时修复目录权限：

    sudo chmod -R a+rX /var/www/course-site

然后重新加载 Nginx：

    sudo systemctl reload nginx

再次访问后，页面样式恢复正常。

## 十、实验结果

本实验成功完成了 Hexo 课程网站在 Ubuntu Server 上的部署。

最终访问地址为：

    http://192.168.56.102/software-tools-practice/

实验结果包括：

- node1 成功安装并运行 Nginx
- Windows 可以访问 node1 的 Web 服务
- 成功创建课程网站部署目录
- 成功配置 Nginx 站点
- 成功生成 Hexo 静态网站
- 成功将 public 文件上传到 node1
- 成功通过 Nginx 发布 Hexo 网站
- 解决了代理访问、403 Forbidden 和 CSS 样式加载问题

## 十一、实验总结

通过本实验，我完整理解了静态网站从本地生成到服务器部署的过程。

Hexo 负责将 Markdown 实验文章生成静态网页，Nginx 负责在服务器上发布这些网页，SSH 和 scp/rsync 负责远程管理和文件同步。

本实验将前面的 Web 运行环境、Linux 环境、远程管理和文件传输内容串联起来，完成了一次完整的网站部署流程。