# mythoshell — Hugo + PaperMod Brand Edition

基于当前线上 Mythoshell 首页升级的完整博客体验版本。保留原有深蓝紫 Hero、Logo、配色和 PaperMod 导航，不修改 `themes/PaperMod`。

## 技术基线

- Hugo Extended 0.170.0
- PaperMod（Git Submodule）
- GitHub Pages + GitHub Actions
- 域名：`https://mythoshell.com/`
- Brand colors：`#3B82F6` / `#8B5CF6` / `#0F172A` / `#94A3B8` / `#F8FAFC`

## 本轮升级内容

- 首页：保留现有 Hero，增加动态技术领域文章数、精选文章、最新文章和 About CTA。
- 文章详情：独立阅读布局、分类/标签、阅读时间、字数、更新时间、桌面侧边 TOC、移动端折叠 TOC、前后篇导航。
- Posts：统一文章卡片、摘要、阅读时间、字数、标签、分页。
- Categories / Tags：术语索引页与单个分类/标签列表页。
- Archives：按年份、月份分组的时间线。
- About：品牌介绍、技术方向、工程理念和侧边品牌卡片。
- Mobile：手机端导航沿用 PaperMod，页面布局、卡片、Hero、TOC、归档和 About 单独适配。

## 1. 环境准备

```bash
brew install hugo git
hugo version
```

确认输出为 Hugo Extended，建议使用 `v0.170.0`。

## 2. 安装 PaperMod 子模块

如果这是新目录：

```bash
git init
git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
git submodule update --init --recursive
```

如果覆盖到现有 Git 仓库，请不要重复 `git init` 或 `git submodule add`；保留原有 `.git` 与 `.gitmodules`，执行：

```bash
git submodule update --init --recursive
```

## 3. 修改个人信息

修改 `hugo.yaml`：

- `socialIcons` 中的 `你的GitHub用户名`
- `editPost.URL` 中的 GitHub 用户名和仓库名

修改 `content/about.md`：补充真实个人介绍和公开联系方式。

## 4. 本地预览与构建

```bash
hugo server -D
```

访问 `http://localhost:1313/`。

生产构建：

```bash
hugo --gc --minify
```

构建产物位于 `public/`。

## 5. 文章 Front Matter 示例

```yaml
---
title: "Redis 缓存一致性实践"
date: 2026-11-21T10:00:00+08:00
draft: false
featured: true
description: "从真实业务场景分析缓存一致性方案与权衡。"
categories: ["Middleware"]
tags: ["Redis", "Cache", "Distributed Systems"]
ShowToc: true
ShowReadingTime: true
ShowWordCount: true
---
```

`featured: true` 会将文章选为首页精选文章。建议同一时间只设置一篇精选文章。

首页技术领域卡片对应分类 slug：

- `Java` → `/categories/java/`
- `Architecture` → `/categories/architecture/`
- `Middleware` → `/categories/middleware/`
- `AI & Agent` → `/categories/ai-agent/`

文章 front matter 中请使用以上分类名称，Hugo 会自动生成 taxonomy 页面。分类卡片的文章数会从 taxonomy 动态读取。

## 6. GitHub Pages 部署

仓库 Settings → Pages → Build and deployment 选择 **GitHub Actions**。

工作流位于 `.github/workflows/deploy.yml`，推送 `main` 分支自动构建部署。`static/CNAME` 已配置 `mythoshell.com`。

## 7. 文件结构

```text
mythoshell-site/
├── .github/workflows/deploy.yml
├── assets/css/extended/mythoshell.css
├── content/
│   ├── about.md
│   ├── archives.md
│   ├── search.md
│   └── posts/
├── layouts/
│   ├── index.html
│   ├── about/single.html
│   ├── archives/single.html
│   ├── posts/list.html
│   ├── _default/
│   │   ├── single.html
│   │   ├── taxonomy.html
│   │   └── terms.html
│   └── partials/
│       ├── extend_head.html
│       └── ms/
│           ├── breadcrumbs.html
│           └── post-card.html
├── static/brand/
├── static/CNAME
├── hugo.yaml
└── README.md
```

## 8. 注意事项

- 不要直接修改 `themes/PaperMod`，自定义模板放在 `layouts/`，样式放在 `assets/css/extended/`。
- `layouts/_default/single.html` 是文章详情模板；`layouts/about/single.html` 仅用于 About 内容类型。
- Hugo taxonomy 页面由 `layouts/_default/terms.html`（分类/标签索引）和 `layouts/_default/taxonomy.html`（具体分类/标签）负责。
- 搜索页继续使用 PaperMod 原生 Search 模板，首页 JSON 输出已保留。
- 若本地 Git 仓库还没有任何 commit，`enableGitInfo: true` 可能导致 lastmod 信息不完整；先创建首次提交再检查。
- 发布前建议运行 `hugo --gc --minify`，检查 taxonomy slug、链接、目录和图片路径。
