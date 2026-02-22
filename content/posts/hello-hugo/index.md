---
title: "欢迎使用 Hugo"
summary: "第一篇示例文章，介绍博客已搭建完成。"
date: 2025-02-22
lastmod: 2025-02-22
categories:
  - 随笔
tags:
  - Hugo
  - 博客
comments: true
ShowToc: true
TocOpen: true
draft: false
---

你好！这是你的第一篇博客文章。

本博客参考 [Dejavu 的教程](https://blog.dejavu.moe/posts/how-i-built-my-personal-blog/) 搭建，使用：

- **Hugo**：快速的静态站点生成器
- **PaperMod**：简洁且功能丰富的主题
- **GitHub**：托管源码
- **Cloudflare Pages / GitHub Pages**：免费部署

## 写新文章

在项目根目录执行：

```bash
hugo new posts/你的文章名/index.md
```

使用 Page Bundle 可以把文章图片放在同一目录下。编辑完成后将 `draft: true` 改为 `draft: false` 即可发布。

## 本地预览

```bash
hugo server -D
```

浏览器打开 http://localhost:1313 即可预览（`-D` 会包含草稿文章）。

祝你写作愉快！
