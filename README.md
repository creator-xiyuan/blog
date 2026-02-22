# 个人博客

基于 [Hugo](https://gohugo.io/) + [PaperMod](https://github.com/adityatelange/hugo-PaperMod) 的静态博客，参考：[我是如何建立自己的个人博客的？](https://blog.dejavu.moe/posts/how-i-built-my-personal-blog/)

### 生成原理（简要）

本站是 **静态站点**：无数据库、无后台，页面在 **构建时** 一次性生成。Hugo 读取 `config.yml`、`content/` 下的 Markdown（含 front matter 元数据）以及主题 `themes/PaperMod`，在本地或 CI 中执行 `hugo`，将内容渲染成完整 HTML，并输出到 `public/`。部署时只需把 `public/` 里的静态文件交给 CDN（如 Cloudflare Pages），用户访问时直接返回这些文件。**baseURL** 必须与最终访问地址一致，否则资源路径会错乱、样式失效。

## 环境准备

- 安装 [Hugo](https://gohugo.io/installation/)（建议 extended 版）
- 安装 Git

## 首次使用（克隆后）

**必须执行**：本仓库通过 Git 子模块引用 PaperMod 主题，克隆后需要拉取主题：

```bash
# 进入项目目录
cd blog

# 拉取 PaperMod 主题（子模块）
git submodule update --init --recursive
```

若你是从零在本地建的仓库、还没有添加过子模块，请先执行：

```bash
git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
```

## 本地写作与预览

```bash
# 启动本地预览（不含草稿）
hugo server

# 含草稿文章
hugo server -D
```

浏览器打开 http://localhost:1313

## 写新文章

```bash
# 使用 Page Bundle，便于同目录放图片
hugo new posts/你的文章名/index.md

# 放在某一层级（章节）下，例如「随笔」「技术」
hugo new posts/随笔/你的文章名/index.md
hugo new posts/技术/你的文章名/index.md
```

编辑对应路径下的 `index.md`，写完后把 front matter 里的 `draft: false` 保留或改为 `false` 即可参与构建。**文章层级**：站点已开启全局目录（TOC），每篇文章会按标题（H2、H3…）显示层级；导航栏的「随笔」「技术」对应 `content/posts/随笔/`、`content/posts/技术/`，新章节可在 `content/posts/` 下新建目录并加 `_index.md`，再在 `config.yml` 的 `menu.main` 里增加一项。

## 构建静态站

```bash
hugo
```

输出在 `public/` 目录。

## 部署

### Cloudflare Pages（推荐，参考原文）

1. 打开 [Cloudflare Pages](https://pages.dev/)，用 GitHub 登录，选择本仓库 `creator-xiyuan/blog`。
2. 构建配置：
   - **框架**：Hugo
   - **环境变量**：`HUGO_VERSION` = `0.128.0`（或你当前使用的版本）
3. 部署后会得到 `xxx.pages.dev` 子域名。若使用自定义域名，在「自定义域名」里添加并按提示解析。

部署完成后，把 `config.yml` 里的 `baseURL` 改成你的 Pages 地址（如 `https://你的项目.pages.dev/`）。

### GitHub Pages（已配置 Actions）

本项目已包含 `.github/workflows/hugo.yml`，推送到 `main` 后会自动用 Hugo 构建并部署到 GitHub Pages。

1. **先添加主题子模块并提交**（若尚未执行）：
   ```bash
   git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
   git add .gitmodules themes/PaperMod
   git commit -m "Add PaperMod theme submodule"
   git push
   ```
2. 在 GitHub 仓库 **Settings → Pages** 中，**Source** 选择 **GitHub Actions**。
3. 之后每次 `git push` 到 `main` 都会自动构建，站点地址为：`https://creator-xiyuan.github.io/blog/`。

`config.yml` 中已预设 `baseURL: 'https://creator-xiyuan.github.io/blog/'`。

## 维护

- **更新主题**：`git submodule update --remote --merge`
- **魔改主题**：不要直接改 `themes/PaperMod`，把要改的模板复制到站点根目录下 `layouts/` 相同路径再修改，详见 [PaperMod FAQ](https://adityatelange.github.io/hugo-PaperMod/posts/papermod/papermod-faq/)。

## 目录结构概览

```
.
├── archetypes/    # 文章模板（如 posts.md）
├── content/       # 文章与页面
│   ├── posts/     # 博客文章（其下 随笔/、技术/ 等为章节层级）
│   │   ├── 随笔/
│   │   └── 技术/
│   └── archives/  # 归档页
├── layouts/       # 自定义布局（覆盖主题）
├── static/        # 静态资源（如图片、favicon）
├── themes/
│   └── PaperMod   # 主题（子模块）
├── config.yml     # 站点配置
└── public/        # 构建输出（已忽略）
```
