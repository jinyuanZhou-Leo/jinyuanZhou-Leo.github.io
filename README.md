# Jinyuan's Blog 锦缘的博客

基于 Hexo 8.1.2 和 Cupertino 2.5.0，使用 Pagefind 生成站内搜索索引。

## 开发环境

使用 Node.js 24.21.0 LTS（见 `.nvmrc`）和 pnpm 12.3.4。macOS 与 Debian 12 使用相同版本；安装 Node.js 后，在项目根目录执行：

```sh
# 已安装 nvm 时，使用 nvm install && nvm use 切换 Node.js。
curl -fsSL https://get.pnpm.io/install.sh | env PNPM_VERSION=12.3.4 sh -
pnpm install --frozen-lockfile
pnpm run clean
pnpm run build
pnpm run index
pnpm run server
```

访问 http://localhost:4000。修改文章后重新运行 `pnpm run build && pnpm run index`，更新搜索索引。

## 主题与依赖

`themes/cupertino` 保留主题源码，并作为 pnpm workspace 与博客共享根目录 `pnpm-lock.yaml`；workspace 成员在 `pnpm-workspace.yaml` 中声明，统一在根目录安装依赖。主题的 Sass 渲染器直接使用 `sass`，不再安装已被它替代的 `hexo-renderer-sass`。

站点设置位于 `_config.yml`，个性化主题设置位于 `_config.cupertino.yml`。`themes/cupertino/layout/post.ejs` 保留本博客的字数和预计阅读时间显示，阅读速度为中文每分钟 300 字、英文每分钟 160 词。后续同步上游主题时保留这处定制。

主题源码来自 [Cupertino v2.5.0](https://github.com/MrWillCom/hexo-theme-cupertino/releases/tag/v2.5.0)，保留上游 LICENSE 和 CHANGELOG；移除仅用于上游 npm 发布的工作流及 Changesets 配置，格式化工具仍可通过 `pnpm --filter hexo-theme-cupertino run format` 使用。

## 发布

推送到 `main` 后，GitHub Actions 使用 `.nvmrc` 指定的 Node.js、pnpm 12.3.4 和 `pnpm install --frozen-lockfile`，构建静态页面及 Pagefind 索引，再发布到 GitHub Pages。
