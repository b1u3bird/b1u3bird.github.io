# Hugo 博客复刻项目

这是一个用 Hugo 搭建的静态博客示例，样式参考 `ddatsh.com`：首页是文章归档表格，文章页包含固定目录、标题信息、中文大字号排版和 Chroma 代码高亮。

## 目录结构

```text
D:\Code\website
├── config.toml                 # Hugo 配置、菜单、taxonomy、代码高亮配置
├── content
│   ├── about.md                # 关于页
│   └── posts                   # 文章内容
├── layouts
│   ├── index.html              # 首页文章归档表格
│   ├── _default
│   │   ├── baseof.html         # 全站 HTML 骨架和导航
│   │   ├── list.html           # 分类、标签、系列列表页
│   │   └── single.html         # 文章页模板
│   └── partials
│       ├── post-category.html  # 从文章目录推导分类
│       └── post-meta.html      # 文章元信息输出
├── static
│   ├── script.js               # 文章目录滚动高亮
│   └── styles.css              # 全站样式
└── public                      # Hugo 构建输出，可删除后重新生成
```

## 常用命令

在项目目录运行：

```powershell
hugo --source "D:\Code\website" --destination "D:\Code\website\public"
```

本地预览：

```powershell
hugo server --source "D:\Code\website" --destination "D:\Code\website\public" --bind 127.0.0.1
```

清理并重新构建：

```powershell
Remove-Item -Recurse -Force "D:\Code\website\public"
hugo --source "D:\Code\website" --destination "D:\Code\website\public"
```

## 部署

这个仓库配置为 GitHub Pages 用户主页站点，线上地址是：

```text
https://b1u3bird.github.io/
```

推送到 `main` 后，GitHub Actions 会自动构建 Hugo 并部署到 Pages。仓库设置里需要把 Pages 的 Source 设为 `GitHub Actions`。

## RSS

RSS 会由 Hugo 自动生成，主页订阅地址是：

```text
/index.xml
```

分类、标签、系列页面也会生成对应 RSS，例如：

```text
/tags/hugo/index.xml
/categories/dev/index.xml
/series/blog-notes/index.xml
```

页面 `<head>` 中已经加入 RSS 自动发现链接，导航栏也包含 `rss` 入口。

## 新增文章

在 `content/posts/` 下按分类目录新建 Markdown 文件。例如：

```text
content/posts/dev/java-memory-model.md
content/posts/tools/clangd-windows.md
content/posts/dev/db/redis-listpack.md
```

首页和文章页的 `categories` 显示来自 `content/posts/` 后面的目录名：

- `content/posts/dev/java-memory-model.md` 显示为 `dev`
- `content/posts/tools/clangd-windows.md` 显示为 `tools`
- `content/posts/dev/db/redis-listpack.md` 显示为 `dev/db`

文章 front matter 只需要保留标题、日期、标签和系列：

```markdown
+++
title = "文章标题"
date = 2026-06-07T09:00:00+08:00
tags = ["hugo", "css"]
series = ["blog notes"]
+++

正文内容。
```

首页只会展示 `content/posts/` 里的文章；`content/about.md` 这类独立页面不会进入首页归档表。

## 样式维护

主要样式在 `static/styles.css`，按这些区域组织：

- `Base`：全局变量、字体、链接基础样式。
- `Layout`：顶部导航、页面宽度、辅助类。
- `Archive`：首页文章表格和列表页。
- `Article`：文章布局、目录、标题块、正文标题和引用块。
- `Code: layout`：代码块容器、字号和字体。
- `Code: chroma tokens`：Chroma 语法高亮颜色。
- `Responsive`：窄屏适配。

代码块字号通过 `.highlight` / `.chroma` 相关规则控制，语法颜色通过 `.chroma` token 类控制。

## 模板说明

- `layouts/index.html`：使用 `where .Site.RegularPages.ByDate.Reverse "Section" "posts"` 只列出文章。
- `layouts/partials/post-category.html`：从 `content/posts/` 后面的目录推导分类名。
- `layouts/partials/post-meta.html`：统一输出分类、标签、字数和日期。
- `layouts/_default/baseof.html`：根据 Hugo 菜单状态给当前导航项添加 `active` 和 `aria-current="page"`。
- `layouts/_default/list.html`：用于 `/categories/`、`/tags/`、`/series/` 等 taxonomy 页面。

## 注意事项

- `public/` 是生成产物，不要手动维护其中的文件。
- 如果改了 CSS 但浏览器看不到变化，检查 `baseof.html` 中 stylesheet 后面的版本参数。
- 当前 Hugo 配置使用 `locale = "zh-cn"`，避免旧版 `languageCode` 的弃用警告。
