# 软件开发工具实践课程展示网站

本项目是《软件开发工具实践》课程第16组的小组实践项目，基于 Hexo 搭建课程展示网站，用于记录课程实验过程、工具使用方法和实践总结。

项目通过 GitHub 管理代码，使用 GitHub Actions 自动部署到 GitHub Pages，同时也将网站部署到 Ubuntu Server 虚拟机中的 Nginx Web 服务器，实现本地服务器访问。

## 一、项目简介

本项目围绕软件开发工具实践课程的主要实验内容展开，完成了从 Web 开发环境搭建、代码仓库配置、Hexo 网站构建，到 Linux 虚拟机环境、SSH 远程管理、文件传输和 Web 站点部署的完整实践流程。

网站内容主要包括：

- Web 开发环境配置
- VS Code 与 Markdown 使用
- Hexo 静态网站构建
- GitHub Pages 自动部署
- VirtualBox 与 Ubuntu Server 虚拟机配置
- Linux 基础命令、权限和环境变量配置
- SSH、scp、rsync 远程管理与文件传输
- Nginx Web 站点部署

## 二、在线访问地址

GitHub Pages 访问地址：

```text
https://nai0181.github.io/software-tools-practice/
```

Ubuntu Server 本地部署访问地址：

```text
http://192.168.56.102/software-tools-practice/
```

说明：Ubuntu Server 部署地址需要在本机 VirtualBox Host-Only 网络环境下访问。

## 三、技术栈

本项目使用的主要工具和技术如下：

- Visual Studio Code：代码与 Markdown 文档编辑
- Node.js：Hexo 运行环境
- npm：Node.js 包管理工具
- Hexo：静态网站生成器
- Markdown：实验文章编写格式
- Git：本地版本控制
- GitHub：远程代码仓库
- GitHub Actions：自动构建与部署
- GitHub Pages：静态网站托管
- VirtualBox：虚拟机管理
- Ubuntu Server：Linux 服务器环境
- OpenSSH：远程登录管理
- scp / rsync：远程文件传输与目录同步
- Nginx：Web 服务器部署

## 四、项目结构说明

```text
software-tools-practice
├── .github
│   └── workflows
│       └── pages.yml
├── scaffolds
├── source
│   └── _posts
├── themes
├── .gitignore
├── README.md
├── _config.yml
├── _config.landscape.yml
├── package.json
└── package-lock.json
```

各部分作用如下：

- `.github`：GitHub Actions 自动化部署配置目录
- `scaffolds`：Hexo 新建文章时使用的模板目录
- `source/_posts`：实验文章 Markdown 文件目录
- `themes`：Hexo 网站主题目录
- `.gitignore`：Git 忽略规则配置文件
- `README.md`：项目说明文档
- `_config.yml`：Hexo 网站主配置文件
- `_config.landscape.yml`：Landscape 主题配置文件
- `package.json`：Node.js 项目依赖说明文件
- `package-lock.json`：锁定 npm 依赖版本，保证环境一致

## 五、实验内容

本项目网站中包含以下实验文章：

1. 实验二 Web 开发环境
2. 实验三 VS Code 与 Markdown
3. 实验四 Hexo 网站构建
4. 实验五 虚拟机安装与使用
5. 实验六 Linux 环境配置
6. 实验七 SSH 远程管理与文件传输
7. 实验八 Web 站点部署

## 六、本地运行方法

首先安装项目依赖：

```bash
npm install
```

启动本地预览服务器：

```bash
hexo clean
hexo server
```

本地访问地址：

```text
http://localhost:4000/software-tools-practice/
```

生成静态网站文件：

```bash
hexo clean
hexo generate
```

生成结果位于：

```text
public/
```

## 七、GitHub Pages 自动部署

本项目配置了 GitHub Actions 自动部署流程。

当代码 push 到 GitHub 仓库的 main 分支后，GitHub Actions 会自动执行以下操作：

```text
拉取项目代码
安装 Node.js 环境
安装 npm 依赖
执行 Hexo 静态网站生成
上传 public 目录
部署到 GitHub Pages
```

自动部署配置文件位于：

```text
.github/workflows/pages.yml
```

## 八、Ubuntu Server 与 Nginx 部署

本项目还将 Hexo 生成的静态网站部署到了 Ubuntu Server 虚拟机中。

部署服务器信息：

```text
服务器名称：node1
服务器 IP：192.168.56.102
部署目录：/var/www/course-site/software-tools-practice/
Web 服务器：Nginx
```

部署流程：

```text
Hexo 生成 public 静态文件
通过 scp 或 rsync 上传到 node1
Nginx 从 /var/www/course-site 目录读取网页文件
Windows 浏览器访问 node1 网站
```

常用部署命令：

```powershell
hexo clean
hexo generate

ssh node1 "rm -rf /var/www/course-site/software-tools-practice/*"

scp -r .\public\* node1:/var/www/course-site/software-tools-practice/

ssh node1 "chmod -R a+rX /var/www/course-site"
```

访问地址：

```text
http://192.168.56.102/software-tools-practice/
```

## 九、虚拟机节点配置

本项目使用 VirtualBox 创建 Ubuntu Server 虚拟机环境，并通过模板机克隆出多个节点。

节点配置如下：

```text
ubuntu-template  192.168.56.101
node1            192.168.56.102
node2            192.168.56.103
node3            192.168.56.104
```

网络配置：

- NAT：用于虚拟机访问外网
- Host-Only：用于 Windows 宿主机与 Ubuntu 虚拟机通信，以及多台虚拟机之间互通

## 十、远程管理与文件传输

本项目配置了 SSH 远程登录，并使用 SSH Key 和 SSH config 简化登录。

示例：

```bash
ssh node1
```

文件传输方式包括：

```text
scp
rsync
```

其中：

- `scp` 适合简单文件复制
- `rsync` 适合目录同步和增量更新
- `rsync --delete` 可以让目标目录与源目录保持一致


## 十一、项目总结

通过本项目，我们完成了一套从本地开发到服务器部署的完整实践流程。

本项目不仅使用 Hexo 搭建了静态课程展示网站，还结合 GitHub Actions 实现了自动化部署，并通过 VirtualBox、Ubuntu Server、SSH、scp、rsync 和 Nginx 完成了本地服务器部署。

通过本次实践，我们对软件开发工具链、Linux 服务器环境、远程管理、文件同步和 Web 站点部署有了更加完整的理解。