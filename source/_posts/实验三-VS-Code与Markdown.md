---
title: 实验三 VS Code与Markdown
date: 2026-07-08 15:27:56
tags:
---
## 一、实验目标

本实验主要学习 Visual Studio Code 编辑器和 Markdown 文档格式的基本使用，掌握使用 VS Code 编辑项目文件、编写 Markdown 文档、预览 Markdown 内容的方法。

<!-- more -->
## 二、核心工具理解

### 1. Visual Studio Code

Visual Studio Code 是一款轻量级代码编辑器，可以通过插件扩展功能。它既可以编辑代码，也可以编辑 Markdown 文档、配置文件和项目说明文件。

在本项目中，VS Code 主要用于打开 Hexo 项目、编辑 Markdown 文章、修改配置文件和查看项目目录结构。

### 2. Markdown

Markdown 是一种轻量级标记语言，可以通过简单符号表示标题、列表、加粗、代码和链接等格式。

相比 Word 文档，Markdown 更适合程序员编写技术文档、项目 README、实验总结和博客文章。

### 3. Markdown 与 Hexo 的关系

在 Hexo 项目中，文章通常以 Markdown 文件的形式保存在 source/_posts 目录下。Hexo 会读取这些 Markdown 文件，并将其转换成 HTML 网页。

因此，Markdown 是内容源文件，Hexo 是网站生成工具。

## 三、常用 Markdown 语法

### 1. 标题

使用 # 表示标题级别。# 越多，标题级别越低。

例如：

    # 一级标题
    ## 二级标题
    ### 三级标题

### 2. 列表

无序列表使用短横线：

    - Node.js
    - npm
    - Git
    - Hexo

有序列表使用数字：

    1. 安装环境
    2. 创建项目
    3. 本地预览
    4. 提交 Git

### 3. 加粗

使用两个星号包裹文字可以表示加粗。

例如：

    **重要内容**

### 4. 行内代码

使用反引号可以表示行内代码。

例如：

    执行 `hexo server` 可以启动本地预览服务。

### 5. 代码块

代码块可以用缩进或三个反引号表示。在实验报告中，代码块适合用来展示命令、配置文件和程序代码。

## 四、实验过程

本实验中，我使用 VS Code 打开 Hexo 项目目录，查看项目结构，并编辑 source/_posts 下的 Markdown 文章。

主要操作包括：

- 打开 Hexo 项目文件夹
- 查看 _config.yml、source、themes 等目录
- 创建新的 Markdown 文章
- 编写实验总结内容
- 使用 Hexo 本地预览网站效果

## 五、实验结果

本实验成功完成了 VS Code 与 Markdown 的基本使用练习，能够使用 VS Code 编辑 Hexo 项目文件，并使用 Markdown 编写结构清晰的实验文章。

## 六、实验总结

通过本实验，我理解了 VS Code、Markdown 和 Hexo 之间的关系。VS Code 是编辑工具，Markdown 是内容格式，Hexo 负责将 Markdown 内容转换为网页。Markdown 语法简单、结构清晰，适合用于编写技术文档和课程实验总结。