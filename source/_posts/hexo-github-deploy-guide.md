---
title: Hexo 博客搭建与 GitHub Pages 部署教程
date: 2026-07-26
tags:
  - Hexo
  - GitHub
  - 博客
categories:
  - 博客搭建学习
    - Hexo
---

## 前言

本文将详细介绍如何使用 Hexo 搭建个人博客，并部署到 GitHub Pages。

## 准备工作

### 1. 安装 Node.js

Hexo 基于 Node.js，需要先安装 Node.js（建议 LTS 版本）。

```bash
# 使用 nvm 安装（推荐）
nvm install 20
nvm use 20
```

验证安装：

```bash
node -v   # 显示 v20.x.x
npm -v    # 显示 10.x.x
```

### 2. 安装 Git

确保已安装 Git，用于版本控制和部署。

```bash
git --version
```

## 第一步：创建 Hexo 项目

### 安装 Hexo CLI

```bash
npm install -g hexo-cli
```

### 初始化项目

```bash
# 创建项目目录
mkdir my-blog && cd my-blog

# 初始化 Hexo 项目
hexo init

# 安装依赖
npm install
```

### 启动本地服务器

```bash
npm run server
```

访问 http://localhost:4000/ 查看效果。

## 第二步：配置 Hexo

### 修改站点配置

编辑 `_config.yml` 文件：

```yaml
# Site
title: 我的博客
subtitle: ''
description: ''
keywords:
author: 你的名字
language: zh-CN
timezone: ''

# URL
url: https://username.github.io
permalink: :year/:month/:day/:title/
```

### 安装主题

这里以 Butterfly 主题为例：

```bash
# 安装主题
npm install hexo-theme-butterfly --save

# 创建主题配置文件
cp node_modules/hexo-theme-butterfly/_config.yml _config.butterfly.yml
```

修改 `_config.yml` 使用 Butterfly 主题：

```yaml
theme: butterfly
```

安装主题依赖：

```bash
npm install hexo-renderer-pug hexo-renderer-stylus --save
```

## 第三步：创建文章

### 创建新文章

```bash
hexo new "文章标题"
```

文章会创建在 `source/_posts/` 目录下，使用 Markdown 格式编写。

### 文章格式

```markdown
---
title: 文章标题
date: 2026-07-26
tags:
  - 标签1
  - 标签2
categories:
  - 分类
---

文章正文...
```

## 第四步：部署到 GitHub Pages

### 创建 GitHub 仓库

1. 登录 GitHub
2. 创建新仓库，命名为 `<username>.github.io`（例如 `zhang1565.github.io`）

### 配置部署

安装部署插件：

```bash
npm install hexo-deployer-git --save
```

修改 `_config.yml` 配置部署：

```yaml
deploy:
  type: git
  repo: https://github.com/username/username.github.io.git
  branch: gh-pages
```

### 首次部署

```bash
npm run deploy
```

部署完成后，访问 https://username.github.io/ 查看博客。

### 后续部署流程

```bash
# 1. 写文章
hexo new "新文章"

# 2. 本地预览
npm run server

# 3. 提交代码到 main 分支
git add .
git commit -m "添加新文章"
git push origin main

# 4. 部署到 GitHub Pages
npm run deploy
```

## 注意事项

### 仓库命名

GitHub Pages 用户名主页必须使用 `<username>.github.io` 作为仓库名，否则访问地址会变成 `https://username.github.io/repo-name/`。

### 分支管理

- `main` 分支：存放源代码
- `gh-pages` 分支：存放静态页面（由 hexo-deployer-git 自动创建）

### 配置 GitHub Pages

在仓库设置页面（Settings > Pages）确认：

- Source: Deploy from a branch
- Branch: gh-pages / (root)

### 缓存问题

如果线上页面未更新，可能是浏览器缓存，按 `Ctrl + Shift + R` 强制刷新。

## 总结

通过以上步骤，你已经成功搭建了一个 Hexo 博客并部署到 GitHub Pages。现在可以开始写文章，分享你的技术心得和生活感悟了！

---

**参考链接**：

- [Hexo 官方文档](https://hexo.io/docs/)
- [Butterfly 主题文档](https://butterfly.js.org/)
- [GitHub Pages 文档](https://docs.github.com/en/pages)
