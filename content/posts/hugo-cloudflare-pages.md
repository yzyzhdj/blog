---
title: "用 Hugo + Cloudflare Pages 搭建这个博客"
date: 2026-11-15T11:30:00+08:00
draft: false
tags: ["Hugo", "Cloudflare", "建站"]
categories: ["技术"]
summary: "从零到上线：Hugo 的目录结构、PaperMod 主题的常用配置，以及如何用 Cloudflare Pages 免费部署并绑定自己的域名。"
---

这个博客由 [Hugo](https://gohugo.io/) 生成、托管在 [Cloudflare Pages](https://pages.cloudflare.com/)。整站是纯静态文件，构建在云端完成，写作和部署的体验都很轻。

下面是我实际使用的流程，供同样想搭个人站点的朋友参考。

## 为什么选这套组合

- **Hugo**：单个二进制文件，构建毫秒级，不需要 Node 生态也能跑；
- **PaperMod**：简洁、支持深色模式、自带站内搜索与目录，中文排版也不错；
- **Cloudflare Pages**：连上 Git 仓库后自动构建，免费额度对个人博客绰绰有余，还自带全球 CDN 与 HTTPS。

## 目录结构

初始化之后的站点结构大致是这样：

```text
blog/
├── hugo.toml            # 站点配置
├── archetypes/          # 新文章模板
├── assets/
│   └── css/extended/    # 自定义样式
├── content/
│   ├── posts/           # 文章
│   ├── about.md         # 关于页
│   ├── archives.md      # 归档页
│   └── search.md        # 搜索页
├── static/              # 原样拷贝到站点根目录的文件
└── themes/PaperMod/     # 主题（直接放在仓库里，无需 submodule）
```

## 本地写作与预览

新建一篇文章：

```bash
hugo new content posts/my-new-post.md
```

启动本地预览（包含草稿）：

```bash
hugo server -D
```

打开 `http://localhost:1313` 就能看到效果，改动会实时刷新。

## 部署到 Cloudflare Pages

### 1. 把代码推到 GitHub

```bash
git init
git add .
git commit -m "chore: init hugo blog"
git branch -M main
git remote add origin https://github.com/<你的用户名>/<仓库名>.git
git push -u origin main
```

### 2. 在 Cloudflare Pages 创建项目

进入 Cloudflare 控制台的 **Workers & Pages → Create → Pages → Connect to Git**，选中刚才的仓库，构建配置填：

| 配置项 | 值 |
| --- | --- |
| Framework preset | Hugo |
| Build command | `hugo --gc --minify` |
| Build output directory | `public` |
| 环境变量 | `HUGO_VERSION` = `0.170.0` |

保存后，每次 `git push` 都会自动构建并发布。

> PaperMod 当前版本要求 Hugo ≥ 0.158。`HUGO_VERSION` 不要留空，否则 Cloudflare 可能使用旧版本导致构建失败。

### 3. 绑定自己的域名

在项目的 **Custom domains** 里添加域名，按提示在 DNS 服务商处加一条 `CNAME` 记录即可。HTTPS 证书会自动签发和续期。

### 4. 修正 baseURL

部署成功后，把 `hugo.toml` 里的 `baseURL` 改成正式域名，再推送一次：

```toml
baseURL = "https://blog.yourdomain.com/"
```

这样 RSS、站点地图和社交卡片的绝对链接才会正确。

## 命令行部署（可选）

不想连 Git 也可以直接上传构建产物：

```bash
hugo --gc --minify
npx wrangler pages deploy public --project-name wang-blog
```

## 日常写作流程

```bash
hugo new content posts/xxx.md   # 1. 新建
hugo server -D                  # 2. 本地预览
git add . && git commit -m "post: xxx"
git push                        # 3. 自动部署
```

就这三步。工具越简单，越容易坚持下去。
