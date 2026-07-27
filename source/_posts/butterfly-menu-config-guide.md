---
title: Butterfly 主题菜单配置详解
date: 2026-07-27
tags:
  - Butterfly
  - 菜单配置
  - 主题设置
categories:
  - 博客搭建学习
    - Butterfly主题
---

## 前言

在使用 Butterfly 主题时，菜单配置是一个很重要的环节。本文将详细介绍如何正确配置 Butterfly 主题的导航菜单，包括菜单格式、路径来源、图标使用以及子菜单配置。

## 菜单配置格式

Butterfly 主题的菜单配置位于 `_config.butterfly.yml` 文件中的 `menu` 部分，基本格式如下：

```yaml
menu:
  菜单项名称: 路径 || 图标
```

### 格式拆解

以 `分類: /categories/ || fas fa-folder-open` 为例：

| 部分 | 值 | 说明 |
|------|-----|------|
| `分類` | 菜单显示名称 | 页面上显示的文字，可以是中文或英文 |
| `/categories/` | 页面路径 | 点击菜单后跳转的 URL 地址 |
| `||` | 分隔符 | 用来分隔路径和图标，前后必须有空格 |
| `fas fa-folder-open` | 图标名称 | Font Awesome 图标库的图标 |

## 页面路径来源

菜单中的路径主要分为两类：Hexo 自动生成的页面和手动创建的自定义页面。

### 一、Hexo 自动生成的页面

这些页面由 Hexo 的插件自动生成，无需手动创建。

| 路径 | 页面名称 | 生成插件 |
|------|----------|----------|
| `/` | 首页 | 默认生成 |
| `/archives/` | 时间轴/归档页 | `hexo-generator-archive` |
| `/categories/` | 分类列表页 | `hexo-generator-category` |
| `/tags/` | 标签列表页 | `hexo-generator-tag` |

这些路径在 `_config.yml` 中也有对应的配置：

```yaml
archive_dir: archives    # 归档页路径
category_dir: categories # 分类页路径
tag_dir: tags            # 标签页路径
```

### 二、自定义页面

如果你需要添加自定义页面（如关于页、友链页），需要先使用命令创建：

```bash
# 创建关于页
hexo new page "about"

# 创建友链页
hexo new page "link"

# 创建音乐页
hexo new page "music"
```

创建后会在 `source/` 目录下生成对应的页面文件，路径即为 `/about/`、`/link/`、`/music/`。

## 图标使用说明

菜单中的图标来自 **Font Awesome** 图标库，图标名称由两部分组成：

```
fas fa-folder-open
├──┘ └────────────┘
前缀   图标名
```

### 图标前缀含义

| 前缀 | 含义 | 风格 |
|------|------|------|
| `fas` | Font Awesome Solid | 实心图标（最常用） |
| `far` | Font Awesome Regular | 空心/轮廓图标 |
| `fab` | Font Awesome Brands | 品牌图标（如 GitHub、Twitter） |

### 常用图标参考

| 图标名称 | 效果 | 用途 |
|----------|------|------|
| `fas fa-home` | 🏠 | 首页 |
| `fas fa-archive` | 📦 | 归档/时间轴 |
| `fas fa-tags` | 🏷️ | 标签 |
| `fas fa-folder-open` | 📂 | 分类 |
| `fas fa-heart` | ❤️ | 关于/喜欢 |
| `fas fa-music` | 🎵 | 音乐 |
| `fas fa-images` | 🖼️ | 照片/图库 |
| `fas fa-video` | 🎬 | 电影 |
| `fas fa-link` | 🔗 | 友链 |
| `fab fa-github` | 🐙 | GitHub |

### 查找更多图标

访问 [Font Awesome 官方网站](https://fontawesome.com/icons) 搜索你需要的图标。

## 子菜单配置

Butterfly 主题支持下拉子菜单，配置格式如下：

```yaml
menu:
  # 父菜单（带下拉）
  父菜单名称||图标:
    子菜单1: 路径 || 图标
    子菜单2: 路径 || 图标
    子菜单3: 路径 || 图标
```

### 注意事项

1. **缩进必须正确**：子菜单必须比父菜单缩进 2 个空格
2. **父菜单格式**：父菜单使用 `名称||图标:` 的格式，后面跟冒号
3. **父菜单没有路径**：父菜单本身不可点击，只作为下拉容器

## 完整配置示例

```yaml
menu:
  # 普通菜单项
  首頁: / || fas fa-home
  時間軸: /archives/ || fas fa-archive
  標籤: /tags/ || fas fa-tags
  分類: /categories/ || fas fa-folder-open
  
  # 下拉子菜单
  清單||fas fa-heartbeat:
    音樂: /music/ || fas fa-music
    照片: /Gallery/ || fas fa-images
    電影: /movies/ || fas fa-video
  
  # 自定义页面
  友鏈: /link/ || fas fa-link
  關於: /about/ || fas fa-heart
```

## 常见错误排查

### 错误1：菜单不显示

- 检查 `menu` 配置项是否正确缩进
- 确保 `||` 前后有空格
- 确认路径对应的页面存在

### 错误2：子菜单不显示

- 检查子菜单的缩进是否正确（必须比父菜单多 2 个空格）
- 确认父菜单格式正确（`名称||图标:` 后面有冒号）

### 错误3：图标显示为方框

- 检查图标名称是否正确
- 确认图标前缀正确（`fas`、`far`、`fab`）

## 总结

Butterfly 主题的菜单配置并不复杂，只要记住以下几点：

1. **格式**：`名称: 路径 || 图标`
2. **路径**：内置页面自动生成，自定义页面需要先创建
3. **图标**：使用 Font Awesome 图标库，注意前缀区分
4. **子菜单**：父菜单用 `名称||图标:`，子菜单正确缩进

通过合理配置菜单，可以让你的博客导航更加清晰和美观！
