---
title: 实验四 Hexo网站构建
categories:
  - 软件开发工具实践
date: 2026-07-08 15:39:38
tags:
---

## 一、实验目标

本实验主要完成 Hexo 静态网站的构建与发布，理解静态网站生成器的工作原理，掌握从 Markdown 内容编写、本地预览、静态文件生成到 GitHub Pages 线上部署的基本流程。

<!-- more -->

## 二、核心概念

### 1. 静态网站

静态网站是由 HTML、CSS、JavaScript 和图片等静态资源组成的网站。访问时，服务器直接返回已经生成好的页面文件，不需要动态查询数据库或实时生成页面。

静态网站具有访问速度快、部署简单、安全性较高等特点，适合用于博客、文档站、课程展示站和个人主页等场景。

### 2. 静态网站生成器

静态网站生成器可以将 Markdown 文档、配置文件和主题模板转换为完整的静态网页文件。

在本实验中，Hexo 就是静态网站生成器。它读取 source/_posts 目录下的 Markdown 文章，结合主题文件，最终生成 public 目录中的静态网站文件。

### 3. Hexo 项目结构

Hexo 项目中常见目录和文件包括：

- _config.yml：网站总配置文件
- source/_posts：博客文章目录
- scaffolds：文章模板目录
- themes：网站主题目录
- package.json：项目依赖说明文件
- public：生成后的静态网站文件目录

其中 source/_posts 是内容源文件目录，public 是 Hexo 生成后的成品目录。

### 4. GitHub Pages

GitHub Pages 是 GitHub 提供的静态网站托管服务，可以将仓库中的静态网页文件发布为可访问的网站。

本项目最终通过 GitHub Pages 部署，线上访问地址为：

    https://nai0181.github.io/software-tools-practice/

### 5. GitHub Actions

GitHub Actions 是 GitHub 提供的自动化工具，可以在代码推送后自动执行指定任务。

在本项目中，GitHub Actions 用于自动安装依赖、生成 Hexo 网站，并将生成后的 public 目录发布到 GitHub Pages。

## 三、实验环境

- 操作系统：Windows 11
- 编辑器：Visual Studio Code
- 运行环境：Node.js
- 包管理器：npm
- 静态网站生成器：Hexo
- 版本控制工具：Git
- 远程仓库平台：GitHub
- 部署平台：GitHub Pages
- 自动化工具：GitHub Actions
- 项目路径：C:\Users\29121\Documents\SoftwareDevToolsPractice\course-site

## 四、实验过程

### 1. 创建 Hexo 网站项目

首先在课程目录下创建 Hexo 项目：

    hexo init course-site
    cd course-site
    npm install

其中，hexo init 用于初始化项目，npm install 用于安装项目依赖。

### 2. 启动本地预览服务

在项目根目录下执行：

    hexo server

然后在浏览器中访问：

    http://localhost:4000

如果能够看到 Hexo 默认页面，说明本地网站运行成功。

### 3. 修改网站基本配置

打开项目根目录下的 _config.yml 文件，修改网站标题、副标题、作者、语言和时区等信息。

配置示例：

    title: 软件开发工具实践展示网站
    subtitle: 基于 Hexo 的课程实验网站
    description: 记录软件开发工具实践课程的实验过程与学习总结
    author: 第16小组
    language: zh-CN
    timezone: Asia/Shanghai

### 4. 创建实验文章

使用 Hexo 命令创建新的实验文章：

    hexo new "实验四 Hexo网站构建"

Hexo 会在 source/_posts 目录下生成对应的 Markdown 文件。文章开头的 Front-matter 用于记录文章标题、日期、标签和分类等信息。

### 5. 设置文章模板

为了统一实验文章格式，修改 scaffolds/post.md 文件，使后续新建文章自动包含实验目标、核心概念、实验环境、实验过程、实验结果、问题与解决方法、实验总结等结构。

这样可以提高小组内容整理的规范性。

### 6. 生成静态网站文件

执行以下命令生成静态网站：

    hexo clean
    hexo generate

其中，hexo clean 用于清理旧的生成文件和缓存，hexo generate 用于重新生成 public 目录中的静态网页文件。

### 7. 配置 GitHub Pages 路径

由于本项目是 GitHub Pages 项目站点，线上地址中包含仓库名 software-tools-practice，因此需要在 _config.yml 中配置：

    url: https://nai0181.github.io/software-tools-practice
    root: /software-tools-practice/

这样可以避免部署后出现页面样式丢失、图片加载失败或链接跳转错误等问题。

### 8. 添加 GitHub Actions 自动部署流程

在 .github/workflows 目录下创建 pages.yml 文件，用于配置自动部署流程。

该流程的主要步骤包括：

- 检出仓库代码
- 安装 Node.js
- 使用 npm ci 安装依赖
- 使用 npx hexo generate 生成网站
- 上传 public 目录
- 部署到 GitHub Pages

### 9. 推送到 GitHub 并触发自动部署

完成修改后，执行 Git 提交流程：

    git add .
    git commit -m "添加GitHub Pages自动部署流程"
    git push

推送到 GitHub 后，GitHub Actions 自动运行，并将 Hexo 网站部署到 GitHub Pages。

## 五、实验结果

本实验成功完成了 Hexo 静态网站的构建与部署。

实验结果包括：

- Hexo 本地网站可以正常运行
- 实验文章可以通过 Markdown 编写并显示在网站中
- 首页文章摘要显示正常
- 项目已经推送到 GitHub 远程仓库
- GitHub Actions 自动部署流程运行成功
- GitHub Pages 线上网站可以正常访问

线上访问地址为：

    https://nai0181.github.io/software-tools-practice/

## 六、遇到的问题与解决方法

### 1. 首页显示整篇文章

问题原因是文章中没有设置摘要分隔符，导致 Hexo 首页直接显示文章全文。

解决方法是在文章实验目标后添加摘要分隔符：

    <!-- more -->

添加后，首页只显示文章摘要，点击文章标题后可以查看完整内容。

### 2. GitHub HTTPS 推送失败

在使用 HTTPS 地址推送 GitHub 时，出现连接被重置的问题。经过排查，判断是当前网络环境对 GitHub Git HTTPS 智能协议链路不稳定。

解决方法是生成 SSH key，将公钥添加到 GitHub，并将 origin 远程地址切换为 SSH 格式：

    git@github.com:nai0181/software-tools-practice.git

之后成功完成 Git 推送。

### 3. GitHub Pages 项目路径问题

GitHub Pages 项目站点的访问路径中包含仓库名。如果 Hexo 中 root 配置不正确，可能导致样式或链接错误。

解决方法是在 _config.yml 中配置正确的 url 和 root：

    url: https://nai0181.github.io/software-tools-practice
    root: /software-tools-practice/

## 七、实验总结

通过本实验，我理解了 Hexo 静态网站构建的基本流程。Hexo 负责将 Markdown 内容转换为静态网页，GitHub 负责保存项目源码，GitHub Actions 负责自动构建和部署，GitHub Pages 负责将生成的网站发布到线上。

本实验让我完整体验了从本地开发、内容编写、版本管理到自动部署上线的流程。相比只在本地运行网站，部署到 GitHub Pages 后，项目成果可以通过公网链接访问，更符合实际软件开发中的项目交付流程。