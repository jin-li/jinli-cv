# 锦鲤简历主题

> **语言切换 / Language Switch**: [English](README.md) | [简体中文](README.zh.md)

基于 Inês Almeida 原创的 [Almeida CV](https://github.com/ineesalmeida/almeida-cv)（MIT 许可证），扩展了多 CV 路由、单 CV 语言/样式控制和新的数据板块的可打印 HTML/CSS 简历模板。

![主页演示](images/screenshot-jinli-cv.png)

**演示站点：**
- 默认页面: [https://jinli-cv-demo.netlify.app/](https://jinli-cv-demo.netlify.app/)
- 德语 CV: [https://jinli-cv-demo.netlify.app/cv-de/](https://jinli-cv-demo.netlify.app/cv-de/)
- 中文 CV: [https://jinli-cv-demo.netlify.app/cv-zh/](https://jinli-cv-demo.netlify.app/cv-zh/)

在线演示由 [cv-demo 仓库](https://github.com/jin-li/cv-demo) 提供支持。

本项目是 [Almeida CV](https://github.com/ineesalmeida/almeida-cv) by Inês Almeida (MIT 许可证) 的衍生作品，由 [jin-li/jinli-cv](https://github.com/jin-li/jinli-cv)（锦鲤简历）维护。

---

## 本分支的新增功能

- **多 CV 支持** 从 `data/<cv-folder>/` 目录生成，每个 CV 通过 `slug` 路由到独立页面
- **单 CV `languageCode`** 用于板块标签和页面语言
- **灵活的板块排序** 通过 `section_order` 和 `side_section_order` 参数
- **单 CV 主题/布局覆盖**（颜色、列宽、间距、板块顺序）
- 新增 **`出版物** 板块
  - 链接/斜体标题支持
  - 列表前的单一时间轴式渐变线
  - 项目符号列表样式和打印友好行为
- 新增 **`项目** 板块
  - 类似经验的视觉样式
  - 每个项目详情列表前的渐变引导线
  - 主题、详情和可选徽标支持
- 新增 **`论文** 板块
  - 类似经验的视觉样式（时间轴、地点图标、项目符号、徽标）
- 更好的子元素间距控制
  - `child_margin` / `child_padding` 现在也适用于列表项
- 修复长板块和出版物列表的打印行为
- 下载按钮通过浏览器打印对话框保存 CV 为 PDF
- 适当的打印边距，防止内容触边

---

## 快速开始（本地开发）

### 环境要求

- **Hugo Extended**（推荐 v0.110.0 或更高版本）
- 安装文档: https://gohugo.io/getting-started/installing/

### 选项 1：运行示例站点（推荐用于测试）

示例站点需要主题位于 `./themes/jinli-cv`。创建软链接指向父目录中的主题：

```bash
# 克隆本仓库
git clone https://github.com/jin-li/jinli-cv.git
cd jinli-cv/exampleSite

# 创建软链接: themes/jinli-cv -> ../.. (主题根目录)
mkdir -p themes
ln -s ../.. themes/jinli-cv

# 启动示例站点
hugo server -D
```

访问 `http://localhost:1313/`。

### 选项 2：从头创建自己的站点

```bash
# 1. 创建新的 Hugo 站点
hugo new site my-cv
cd my-cv

# 2. 初始化 git
git init

# 3. 将本主题添加为 git 子模块
git submodule add https://github.com/jin-li/jinli-cv.git themes/jinli-cv

# 4. 将示例站点内容复制到站点根目录
cp -r themes/jinli-cv/exampleSite/* .

# 5. （可选）移除示例的 git 跟踪
rm -rf .git

# 6. 启动开发服务器
hugo server -D
```

您的站点结构将如下所示：
```
my-cv/
├── config.toml              # 站点配置
├── content/
│   └── _content.gotmpl      # 动态 CV 页面生成器
├── data/
│   ├── content.yaml         # 默认 CV（主页）
│   ├── cv-de/
│   │   ├── config.toml
│   │   └── content.yaml
│   └── cv-zh/
│       ├── config.toml
│       └── content.yaml
├── static/
│   └── img/
│       └── avatar.png
└── themes/
    └── jinli-cv/            # 主题子模块
```

---

## 自定义您的 CV

### 1. 编辑默认 CV（主页）

编辑 `data/content.yaml` 填写您的信息。参见 `themes/jinli-cv/exampleSite/data/content.yaml` 获取所有可用字段。

**主要板块：**
```yaml
Name:
  first: Your
  last: Name
  order: first_last      # 或 last_first
  align: center          # left, center, right

Avatar:
  Photo: https://your-avatar-url.com/photo.jpg

Contacts:
  - Icon: fas fa-envelope
    Info: your@email.com
  - Icon: fas-globe
    Info: <a class="contact__link" href="https://your-site.com" target="_blank">your-site.com</a>

Profile: "您的专业摘要..."

Experience:
  - Employer: 公司名称
    Place: 城市, 国家
    Positions:
      - Title: 高级开发工程师
        Date: 2022 - 至今
        Details:
          - 成就 1
          - 成就 2
        Badges: ["Go", "Docker", "Kubernetes"]

Education:
  - Course: 计算机科学硕士
    Institution: 大学名称
    Date: 2020 - 2022
    Details:
      - Thesis: "您的论文标题"
```

### 2. 添加额外的 CV（多语言、不同版本）

为每个 CV 在 `data/` 下创建文件夹：

```bash
mkdir -p data/cv-de data/cv-fr
```

每个文件夹需要 **两个文件**：

**`data/cv-de/config.toml`**：
```toml
slug = "cv-de"
title = "Lebenslauf"
languageCode = "de"

[params]
# 可覆盖任何全局参数
section_order = ["profile", "experience", "projects", "education", "skills"]
side_section_order = ["avatar", "name", "contacts", "languages", "interests"]
showDownload = true
download_button = "top_right"
```

**`data/cv-de/content.yaml`**：
```yaml
Name:
  first: Dein
  last: Name
  order: first_last
  align: center

Avatar:
  Photo: https://your-avatar-url.com/photo.jpg

Contacts:
  - Icon: fas fa-envelope
    Info: dein@email.de

Profile: "Dein professionelles Profil auf Deutsch..."

Experience:
  - Employer: Firmenname
    Place: Berlin, Deutschland
    Positions:
      - Title: Senior Entwickler
        Date: 2022 - Heute
        Details:
          - Erfolg 1
          - Erfolg 2
        Badges: ["Go", "Docker", "Kubernetes"]
```

### 3. 替换头像

用您的照片替换 `static/img/avatar.png`（推荐：正方形，约 400x400px）。

### 4. 自定义颜色和布局

在 `config.toml` 的 `[params]` 下修改以覆盖主题默认值：

```toml
[params]
colorPrimary = "#您的品牌颜色"
colorLight = "#fff"
colorDark = "#333"
colorPageBackground = "#f5f5f5"
# ... 参见 exampleSite/config.toml 获取所有选项
```

### 5. 添加自定义 SCSS（高级）

在站点根目录创建 `assets/scss/_custom.scss`：

```scss
// 覆盖主题变量
$color-primary: #your-color;
$font-family-base: 'Your Font', sans-serif;

// 自定义样式
.your-custom-class {
  // ...
}
```

---

## 生产环境构建

```bash
# 清理构建
hugo --cleanDestinationDir --minify

# 输出将在 ./public/ 目录
# 将 public/ 目录的内容部署到您的托管服务提供商
```

---

## 部署指南

### 🌐 部署到 Netlify（推荐 - 免费套餐）

**选项 A：连接您的 Git 仓库（最简单）**

1. 将站点推送到 Git 仓库（GitHub、GitLab、Bitbucket）
2. 访问 [Netlify](https://app.netlify.com/) → **添加新站点** → **导入现有项目**
3. 选择您的仓库
4. 配置构建设置：
   - **基础目录**：留空（或如果直接克隆主题仓库，则为 `themes/jinli-cv/exampleSite`）
   - **构建命令**：`hugo --minify`（或 `hugo --gc --minify`）
   - **发布目录**：`public`
5. 添加环境变量：`HUGO_VERSION = 0.146.5`（或您首选的版本）
6. 点击 **部署站点**

**选项 B：使用 `netlify.toml`（推荐用于可重现性）**

在站点根目录创建 `netlify.toml`：

```toml
[build]
  command = "hugo --minify"
  publish = "public"

[build.environment]
  HUGO_VERSION = "0.146.5"

# 可选：将 www 重定向到非 www
[[redirects]]
  from = "https://www.yourdomain.com/*"
  to = "https://yourdomain.com/:splat"
  status = 301
  force = true
```

**如果使用主题作为子模块**，添加到 `netlify.toml`：

```toml
[build]
  command = "git submodule update --init --recursive && hugo --minify"
  publish = "public"
```

Netlify 将自动：
- 克隆您的仓库
- 初始化子模块（获取主题）
- 运行 Hugo 构建
- 部署到 `https://your-site.netlify.app`
- 每次 git 推送时设置持续部署

**自定义域名：** Netlify DNS → 添加自定义域名 → 按照验证步骤操作。

---

### 🐙 部署到 GitHub Pages（免费）

**选项 A：GitHub Actions（推荐）**

创建 `.github/workflows/hugo.yml`：

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main, master]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: true

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive  # 主题子模块需要
          fetch-depth: 0

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v3
        with:
          hugo-version: '0.146.5'
          extended: true

      - name: Build
        run: hugo --minify --gc

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

然后在仓库设置中：
1. 转到 **设置** → **Pages**
2. **来源**："GitHub Actions"
3. 您的站点将位于 `https://yourusername.github.io/your-repo-name/`

**选项 B：手动部署到 `gh-pages` 分支**

```bash
# 构建
hugo --minify --gc

# 部署到 gh-pages 分支
cd public
git init
git add .
git commit -m "Deploy"
git push -f https://github.com/yourusername/your-repo.git master:gh-pages
```

---

### ☁️ 部署到 Cloudflare Pages（免费）

1. 访问 [Cloudflare Pages](https://pages.cloudflare.com/) → **创建项目**
2. 连接您的 Git 仓库
3. 构建设置：
   - **构建 command**：`hugo --minify`
   - **构建输出目录**：`public`
   - **根目录**：留空（或如果使用子文件夹则设置）
4. 环境变量：`HUGO_VERSION = 0.146.5`
5. 部署！

---

### 🚀 部署到 Vercel（免费套餐）

1. 访问 [Vercel](https://vercel.com/) → **添加新项目**
2. 导入您的 Git 仓库
3. 框架预设：**Hugo**
4. 构建设置：
   - **构建 Command**：`hugo --minify`
   - **输出目录**：`public`
5. 部署！

---

### 🐳 Docker 部署（任意 VPS/云）

**Dockerfile:**
```dockerfile
FROM klakegg/hugo:0.146.5-ext AS builder
WORKDIR /src
COPY . .
RUN hugo --minify

FROM nginx:alpine
COPY --from=builder /src/public /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

**nginx.conf:**
```nginx
server {
    listen 80;
    server_name localhost;
    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    # 启用 gzip
    gzip on;
    gzip_types text/css application/javascript;

    # 缓存静态资源
    location ~* \.(css|js|png|jpg|jpeg|gif|ico|svg|woff|woff2)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
}
```

构建和运行：
```bash
docker build -t my-cv .
docker run -p 8080:80 my-cv
```

---

## 多 CV 路由详情

主题会自动为 `data/` 中包含 `config.toml` 和 `content.yaml` 的每个文件夹生成页面。

| 文件夹 | `config.toml` 中的 `slug` | URL |
|--------|----------------------|-----|
| `data/` (根目录) | N/A | `/` (主页) |
| `data/cv-de/` | `cv-de` | `/cv-de/` |
| `data/cv-zh/` | `cv-zh` | `/cv-zh/` |
| `data/cv-fr/` | `cv-fr` | `/cv-fr/` |

### 单 CV 配置

每个 `data/<cv>/config.toml` 可以覆盖：

```toml
slug = "cv-fr"           # 必填：URL 路径
title = "CV Français"    # 页面标题
languageCode = "fr"      # i18n 语言代码

[params]
# 任何全局参数都可以按 CV 覆盖
section_order = [...]
side_section_order = [...]
showDownload = true
colorPrimary = "#different-color"
# ... 等等
```

---

## 打印 / PDF 导出

主题专为 **A4 打印** 设计：

1. 在浏览器中打开您的 CV
2. 按 `Ctrl+P`（Mac 为 `Cmd+P`）
3. 设置：
   - **纸张大小**：A4
   - **页边距**：无/默认（内置 0.5cm 上下页边距）
   - **背景图形**：**启用**（徽标/渐变色需要）
   - **缩放**：100%

点击 **另存为 PDF** 生成打印就绪的 PDF。

---

## i18n（翻译）

板块标签通过 `i18n/*.toml` 翻译。当前支持的语言：

| 代码 | 语言 | 文件 |
|------|------|------|
| `en` | 英语 | `i18n/en.toml` |
| `de` | 德语 | `i18n/de.toml` |
| `es` | 西班牙语 | `i18n/es.toml` |
| `eo` | 世界语 | `i18n/eo.toml` |
| `fr` | 法语 | `i18n/fr.toml` |
| `pl` | 波兰语 | `i18n/pl.toml` |
| `zh-cn` | 简体中文 | `i18n/zh-cn.toml` |

添加语言：
1. 复制 `i18n/en.toml` 到 `i18n/your-code.toml`
2. 翻译所有值
3. 在 CV 配置中使用 `languageCode = "your-code"`

---

## 主题结构

```
jinli-cv/
├── assets/
│   └── scss/              # SCSS 源文件
├── exampleSite/           # 完整的示例站点
│   ├── config.toml
│   ├── content/
│   ├── data/
│   └── static/
├── i18n/                  # 翻译文件
├── images/                # README 截图
├── layouts/
│   ├── cv/                # CV 页面模板
│   ├── partials/          # 可重用组件
│   └── _default/          # 基础模板
├── static/                # 静态资源（字体等）
├── theme.toml             # 主题元数据
└── LICENSE                # MIT 许可证
```

---

## 故障排除

### Hugo 版本问题
```bash
# 检查版本
hugo version

# 安装特定版本（macOS）
brew install hugo@0.146.5

# 或从 https://github.com/gohugoio/hugo/releases 下载
```

### Netlify/Vercel 子模块未初始化
确保构建命令包含子模块初始化：
```toml
# netlify.toml
[build]
  command = "git submodule update --init --recursive && hugo --minify"
```

### 主题未找到错误
如果看到 `module "jinli-cv" not found`，添加到 `config.toml`：
```toml
themesDir = "themes"
theme = "jinli-cv"
```

### 打印样式不生效
- 在打印对话框中启用 **背景图形**
- 检查浏览器打印预览是否与屏幕一致
- 确保 `hugo --minify` 不会剥离关键 CSS（它不应该）

---

## 贡献

1. Fork 本仓库
2. 创建功能分支：`git checkout -b feature/my-feature`
3. 提交更改：`git commit -am '添加我的功能'`
4. 推送：`git push origin feature/my-feature`
5. 发起 Pull Request

欢迎提交 Issues 和 Pull Requests！

---

## 致谢

- **原始主题**：[Almeida CV](https://github.com/ineesalmeida/almeida-cv) 由 Inês Almeida 创作 (MIT 许可证)
- **当前主题**：[锦鲤简历](https://github.com/jin-li/jinli-cv) 由 Jin Li 维护 (MIT 许可证)
- **字体**：Oswald、Roboto、Material Icons、Font Awesome

---

## 许可证

MIT 许可证 - 详见 [LICENSE](LICENSE)。

Copyright (c) 2020 Inês Almeida
Copyright (c) 2024-2026 Jin Li