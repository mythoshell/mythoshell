# mythoshell — Hugo + PaperMod + GitHub Pages

品牌化个人技术博客项目。域名：`mythoshell.com`。

## 技术栈

- Hugo Extended 0.170.0（PaperMod 当前最低要求为 0.146.0）
- PaperMod（Git submodule）
- GitHub Pages + GitHub Actions
- 自定义首页模板 + `assets/css/extended/mythoshell.css`

## 1. 准备环境（macOS）

```bash
brew install hugo git
hugo version
```

确保输出包含 `extended`，并且版本不低于 `v0.146.0`；CI 当前固定使用 `0.170.0`。

## 2. 初始化主题

解压本项目后进入目录：

```bash
cd mythoshell-site
git init
git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
git submodule update --init --recursive
```

如果你从 GitHub 克隆已存在的仓库：

```bash
git clone --recurse-submodules https://github.com/<你的用户名>/mythoshell-blog.git
```

## 3. 本地预览

```bash
hugo server -D
```

浏览器打开 `http://localhost:1313/`。

正式构建：

```bash
hugo --gc --minify
```

构建产物位于 `public/`。

## 4. 个性化配置（必须修改）

编辑 `hugo.yaml`：

1. 把 `socialIcons` 中的 `你的GitHub用户名` 改成真实 GitHub 用户名。
2. 把 `editPost.URL` 中的用户名和仓库名改成实际值。
3. 修改 `content/about.md` 中的个人介绍、GitHub 和邮箱。
4. 如不希望显示 GitHub 社交入口，可删除对应 `socialIcons` 条目。

## 5. 创建 GitHub 仓库并推送

建议仓库名：`mythoshell-blog`（公开仓库最方便使用 GitHub Pages）。

```bash
git add .
git commit -m "feat: initialize mythoshell blog"
git branch -M main
git remote add origin https://github.com/<你的用户名>/mythoshell-blog.git
git push -u origin main
```

注意：PaperMod 是 Git submodule，`.gitmodules` 已提供地址；首次本地初始化时执行 `git submodule add`，不要把主题目录作为普通文件复制提交。

## 6. 配置 GitHub Pages

GitHub 仓库 → Settings → Pages：

- Build and deployment：选择 **GitHub Actions**。
- Workflow 会在推送 `main` 分支时运行 `.github/workflows/deploy.yml`。
- 部署成功后，在 Custom domain 填入 `mythoshell.com` 并保存。
- 勾选 Enforce HTTPS（证书签发可能需要等待 DNS 生效）。

## 7. 配置 DNS

在域名注册商 DNS 管理面板配置：

| 类型 | 主机记录 | 指向 |
|---|---|---|
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |
| CNAME | www | `<你的用户名>.github.io` |

如果注册商支持 AAAA，也可按 GitHub Pages 官方文档添加 IPv6 记录。不要把 `www` CNAME 指向 `mythoshell.com`，推荐直接指向 GitHub Pages 用户域名。

仓库 `static/CNAME` 已写入 `mythoshell.com`，构建时会复制到站点根目录。DNS 生效后等待 GitHub Pages 检查并签发 HTTPS 证书。

## 8. 发布文章

```bash
hugo new posts/redis-cache-design.md
```

编辑生成的 `content/posts/redis-cache-design.md`，确认 front matter 中 `draft: false` 或删除 `draft`，然后：

```bash
git add content/posts/redis-cache-design.md
git commit -m "docs: add redis cache design"
git push
```

GitHub Actions 自动构建并发布。草稿默认不会出现在生产站点；本地可用 `hugo server -D` 预览草稿。

## 9. 目录结构

```text
mythoshell-site/
├── .github/workflows/deploy.yml  # GitHub Pages 自动部署
├── assets/css/extended/          # PaperMod 扩展样式
├── content/
│   ├── _index.md
│   ├── about.md
│   ├── archives.md
│   ├── search.md
│   └── posts/
├── layouts/
│   ├── index.html                # 品牌化首页
│   └── partials/extend_head.html
├── static/
│   ├── CNAME
│   └── brand/                    # 交付的品牌 SVG / PNG / Favicon
├── themes/PaperMod/              # Git submodule，不随源码压缩包重复打包
├── .gitmodules
├── .gitignore
└── hugo.yaml
```

## 10. 常见问题

- **首页显示默认文章列表**：检查 `layouts/index.html` 是否存在，主题子模块是否初始化。
- **本地样式不生效**：确认 `assets/css/extended/mythoshell.css` 路径正确，清理缓存后重启 `hugo server`。
- **搜索无结果**：检查 `hugo.yaml` 中 `outputs.home` 是否包含 `JSON`，并确认 PaperMod 的搜索配置与页面布局存在。
- **部署找不到主题**：检查 GitHub Actions checkout 是否设置 `submodules: recursive`。
- **自定义域名 404 / HTTPS 不可用**：检查 DNS、`static/CNAME`、仓库 Pages Custom domain 是否一致；等待 DNS 与证书生效。

## 11. 维护建议

- 不要直接修改 `themes/PaperMod`，通过 `layouts/`、`assets/css/extended/` 覆盖。
- 定期更新 PaperMod 子模块：`git -C themes/PaperMod pull`，然后提交子模块指针变化。
- 每篇文章尽量包含摘要、分类、标签、代码示例和实践结论。
- 可在后续阶段增加评论系统（如 Giscus）、站点统计和 RSS 订阅，不建议第一版引入不必要的复杂度。
