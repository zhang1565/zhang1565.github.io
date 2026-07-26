# zhang的博客

基于 Hexo + Butterfly 主题搭建的个人博客。

## 技术栈

- **框架**: Hexo
- **主题**: Butterfly
- **部署**: GitHub Pages

## 快速开始

### 安装依赖

```bash
npm install
```

### 开发阶段

启动本地服务器预览博客，修改后实时查看效果：

```bash
npm run server
```

访问: http://localhost:4000/

### 提交代码

```bash
# 1. 查看修改内容
git status

# 2. 添加所有修改的文件
git add .

# 3. 提交代码（写清楚修改内容）
git commit -m "你的修改描述"

# 4. 推送到远程 main 分支
git push origin main
```

### 部署上线

构建并部署到 GitHub Pages：

```bash
npm run deploy
```

访问: https://zhang1565.github.io/

## 完整工作流程

```bash
# 日常写作流程：
1. 写文章（在 source/_posts/ 目录下创建 .md 文件）
2. npm run server  # 本地预览
3. git add .       # 添加修改
4. git commit -m "添加新文章"  # 提交
5. git push origin main  # 推送到远程
6. npm run deploy  # 部署上线
```

## 文件结构

```
.
├── source/           # 源文件目录
│   └── _posts/       # 文章目录
├── themes/           # 主题目录
│   └── butterfly/    # Butterfly 主题
├── _config.yml       # Hexo 配置文件
├── _config.butterfly.yml  # Butterfly 主题配置文件
└── package.json      # 项目依赖配置
```

## 配置说明

- `_config.yml`: 网站基本信息、URL、部署配置等
- `_config.butterfly.yml`: Butterfly 主题配置（导航、社交媒体、头像等）

## 主题文档

Butterfly 主题详细配置文档：https://butterfly.js.org/
