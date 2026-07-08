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

## 二、核心工具理解

### 1. Node.js

Node.js 是 JavaScript 的运行环境。它让 JavaScript 不仅可以在浏览器中运行，也可以在本地命令行和服务器环境中运行。

在本实验中，Hexo 依赖 Node.js 运行，因此需要先确保 Node.js 已正确安装。

### 2. npm

npm 是 Node.js 自带的包管理器，用于安装和管理项目依赖。

执行 `npm install` 时，npm 会根据项目中的 `package.json` 文件下载当前项目需要的依赖，并将依赖安装到 `node_modules` 目录中。

### 3. Git

Git 是版本控制工具，用于记录项目的修改历史，方便后续进行版本回退、多人协作和远程部署。

在后续实验中，Git 还会用于将本地项目推送到 GitHub 仓库。

### 4. Hexo

Hexo 是一个静态网站生成器。它可以把 Markdown 文件转换成 HTML、CSS、JavaScript 等静态网页文件。

本课程中使用 Hexo 搭建课程实践展示网站，用于展示实验过程、实验截图和学习总结。

## 三、实验过程

### 1. 检查本机开发环境

首先在 PowerShell 中执行以下命令，检查 Node.js、npm、Git、Hexo 和 VS Code 是否安装成功：

```bash
node -v
npm -v
git --version
hexo -v
code --version
```

如果以上命令能够正常输出版本号，说明基础开发环境已经配置完成。

### 2. 创建课程项目目录

在文档目录下创建课程项目文件夹：

```powershell
mkdir "$HOME\Documents\SoftwareDevToolsPractice"
cd "$HOME\Documents\SoftwareDevToolsPractice"
```

该目录用于存放本课程相关的项目文件和实验内容。

### 3. 初始化 Hexo 项目

在课程目录下执行以下命令，创建 Hexo 网站项目：

```bash
hexo init course-site
cd course-site
npm install
```

其中：

- `hexo init course-site` 用于创建一个名为 `course-site` 的 Hexo 项目；
- `cd course-site` 用于进入项目目录；
- `npm install` 用于安装项目所需依赖。

### 4. 启动本地预览服务

在 Hexo 项目根目录下执行：

```bash
hexo server
```

或者使用简写：

```bash
hexo s
```

启动成功后，终端会显示类似信息：

```text
Hexo is running at http://localhost:4000/
```

然后在浏览器中访问：

```text
http://localhost:4000
```

如果能够看到 Hexo 默认页面，说明本地网站启动成功。

### 5. 修改网站基本配置

打开项目根目录下的 `_config.yml` 文件，修改网站标题、描述、作者等基本信息：

```yaml
title: 软件开发工具实践展示网站
subtitle: 基于 Hexo 的课程实验网站
description: 记录软件开发工具实践课程的实验过程与学习总结
author: 第16组
language: zh-CN
timezone: Asia/Shanghai
```

保存后刷新浏览器，可以看到网站标题发生变化。

### 6. 创建实验文章

在项目根目录下执行：

```bash
hexo new "实验二 Web开发环境"
```

Hexo 会在 `source/_posts` 目录下自动生成一篇 Markdown 文章。

生成的文章文件中，开头的内容如下：

```markdown
---
title: 实验二 Web开发环境
date: 2026-07-07 19:30:00
tags:
---
```

这一部分叫 Front-matter，用于记录文章标题、日期、标签和分类等信息。真正的正文内容需要写在第二个 `---` 下面。

## 四、实验结果

本实验成功完成了 Web 软件开发环境的配置，并成功运行了本地 Hexo 网站。

实验结果包括：

- Node.js 可以正常运行；
- npm 可以正常管理项目依赖；
- Git 可以正常使用；
- Hexo CLI 安装成功；
- Hexo 项目初始化成功；
- 本地网站可以通过 `http://localhost:4000` 访问；
- 成功创建并编辑了第一篇实验文章。

## 五、实验总结

通过本实验，我理解了 Web 开发环境中各类工具的作用。

Node.js 提供 JavaScript 运行环境，npm 负责安装和管理项目依赖，Git 用于项目版本控制，Hexo 则负责将 Markdown 内容构建为静态网站。

整个实验过程体现了现代 Web 项目从环境准备、项目初始化、本地运行到内容编写的基本流程，为后续的网站构建、版本管理和部署实验打下了基础。