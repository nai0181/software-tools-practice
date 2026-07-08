---
title: 实验二 Web开发环境
date: 2026-07-07 19:30:00
tags:
  - Node.js
  - npm
  - Git
  - Hexo
categories:
  - 软件开发工具实践
---

## 一、实验目标

本实验主要完成 Web 软件开发环境的安装与配置，掌握 Node.js、npm、Git 和 Hexo CLI 等工具的基本作用，并成功创建一个本地 Hexo 网站项目。

<!-- more -->

## 二、核心概念

Node.js 是 JavaScript 的运行环境，使 JavaScript 可以在本地命令行和服务器环境中运行。Hexo 依赖 Node.js 运行。

npm 是 Node.js 的包管理器，用于安装和管理项目依赖。执行 npm install 时，npm 会根据 package.json 下载依赖，并生成 node_modules 目录。

Git 是版本控制工具，用于记录项目修改历史。它可以保存项目的不同版本，并支持后续推送到 GitHub 远程仓库。

Hexo 是静态网站生成器，可以将 Markdown 文章转换成 HTML、CSS、JavaScript 等静态网页文件。

## 三、实验环境

- 操作系统：Windows 11
- 编辑器：Visual Studio Code
- 运行环境：Node.js
- 包管理器：npm
- 版本控制工具：Git
- 静态网站生成器：Hexo
- 项目路径：C:\Users\29121\Documents\SoftwareDevToolsPractice\course-site

## 四、实验过程

首先检查本机开发环境，确认 Node.js、npm、Git、Hexo 和 VS Code 均可以正常使用。

然后在课程目录下使用 Hexo 初始化网站项目，进入项目目录后执行 npm install 安装依赖。

项目创建完成后，使用 hexo server 启动本地预览服务，并在浏览器中访问 http://localhost:4000 查看网站效果。

之后修改 _config.yml 文件，配置网站标题、副标题、作者、语言和时区等基本信息。

最后使用 hexo new 创建实验文章，并在 source/_posts 目录下编辑 Markdown 内容。

## 五、实验结果

本实验成功完成了 Web 开发环境配置，Hexo 项目可以正常运行，本地网站可以通过 http://localhost:4000 访问。

同时，项目已经完成 Git 本地仓库初始化，并成功推送到 GitHub 远程仓库。

## 六、遇到的问题与解决方法

实验过程中曾出现项目目录嵌套的问题，原因是在已有 course-site 项目目录中再次执行了 hexo init course-site。后来通过回到外层正确项目目录运行 Hexo，解决了该问题。

Git 推送 GitHub 时，HTTPS 方式出现连接重置问题。后来改用 SSH 方式连接 GitHub，并成功完成 push。

## 七、实验总结

通过本实验，我理解了 Web 开发环境中各类工具的作用。Node.js 提供运行环境，npm 管理依赖，Git 负责版本控制，Hexo 负责将 Markdown 内容构建为静态网站。

本实验让我初步掌握了从环境配置、项目初始化、本地运行到 GitHub 推送的完整基础流程，为后续网站构建和部署实验打下了基础。

通过本实验，我理解了 Web 开发环境中各类工具的作用。

Node.js 提供 JavaScript 运行环境，npm 负责安装和管理项目依赖，Git 用于项目版本控制，Hexo 则负责将 Markdown 内容构建为静态网站。

整个实验过程体现了现代 Web 项目从环境准备、项目初始化、本地运行到内容编写的基本流程，为后续的网站构建、版本管理和部署实验打下了基础。