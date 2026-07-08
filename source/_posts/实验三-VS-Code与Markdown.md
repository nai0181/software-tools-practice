---
title: 实验三 VS Code与Markdown
date: 2026-07-08 15:27:56
tags:
---
## 一、实验目标

本实验主要学习 Visual Studio Code 编辑器和 Markdown 文档格式的基本使用，掌握使用 VS Code 编辑项目文件、编写 Markdown 文档和预览网站内容的方法。

<!-- more -->

## 二、核心概念

Visual Studio Code 是一款轻量级代码编辑器，可以通过插件扩展功能。它可以用于编辑代码、Markdown 文档、配置文件和项目说明文件。

Markdown 是一种轻量级标记语言，可以使用简单符号表示标题、列表、加粗、代码和链接等格式。它适合编写技术文档、README、实验总结和博客文章。

在 Hexo 项目中，Markdown 是内容源文件。Hexo 会读取 source/_posts 目录下的 Markdown 文件，并将其转换成网页。

## 三、实验环境

- 操作系统：Windows 11
- 编辑器：Visual Studio Code
- 项目类型：Hexo 静态网站
- 文档格式：Markdown
- 项目路径：C:\Users\29121\Documents\SoftwareDevToolsPractice\course-site

## 四、实验过程

首先使用 VS Code 打开 Hexo 项目根目录，查看项目中的 _config.yml、source、themes、scaffolds 等目录和文件。

然后在 source/_posts 目录下编辑 Markdown 文章，使用标题、列表、加粗、行内代码等 Markdown 语法组织实验内容。

接着通过 hexo server 启动本地预览服务，在浏览器中访问 http://localhost:4000 查看文章显示效果。

为了避免首页显示整篇文章，在实验文章中添加了摘要分隔符，使首页只显示文章摘要，点击文章标题后再查看完整正文。

## 五、实验结果

本实验成功完成了 VS Code 与 Markdown 的基本使用练习，能够使用 VS Code 编辑 Hexo 项目文件，并使用 Markdown 编写结构清晰的实验文章。

文章内容可以被 Hexo 正确解析并显示在本地网站中。

## 六、遇到的问题与解决方法

实验过程中发现首页会直接显示整篇文章。原因是文章中没有设置摘要分隔符。通过在实验目标后添加摘要分隔符，解决了首页显示过长的问题。

此外，在复制 Markdown 内容时需要注意代码块标记，避免将说明文字误放入代码块中。

## 七、实验总结

通过本实验，我理解了 VS Code、Markdown 和 Hexo 之间的关系。VS Code 是编辑工具，Markdown 是内容格式，Hexo 负责将 Markdown 内容转换为网页。

Markdown 语法简单、结构清晰，非常适合用于编写课程实验记录、项目 README 和技术博客。

通过本实验，我理解了 VS Code、Markdown 和 Hexo 之间的关系。VS Code 是编辑工具，Markdown 是内容格式，Hexo 负责将 Markdown 内容转换为网页。Markdown 语法简单、结构清晰，适合用于编写技术文档和课程实验总结。