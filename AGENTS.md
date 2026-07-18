# MyBlog 项目说明

这是一个基于 Hexo + Fluid 主题的个人博客项目。

## 项目信息

- 框架：Hexo 7.3.0
- 主题：hexo-theme-fluid 1.9.8
- 语言：zh-CN
- 部署目标：GitHub Pages
- 博客地址：https://silvermars.github.io/blog
- GitHub 仓库：https://github.com/silvermars/blog

## 常用命令

```bash
# 本地预览
npx hexo server

# 生成静态站点
npx hexo generate

# 清理缓存并重新生成
npx hexo clean && npx hexo generate

# 部署到 GitHub Pages（推送到 gh-pages 分支）
npx hexo deploy

# 完整流程：清理、生成、部署
npx hexo clean && npx hexo generate && npx hexo deploy
```

## 目录结构

- `source/_posts/`：博客文章（Markdown 文件）
- `source/about/`：关于页面
- `source/images/`：文章引用的图片
- `themes/`：主题目录（当前通过 npm 安装 Fluid，本地只保留 landscape）
- `_config.yml`：Hexo 主配置
- `_config.fluid.yml`：Fluid 主题配置
- `public/`：生成的静态站点（`.gitignore` 忽略，不要提交）
- `.deploy_git/`：`hexo deploy` 的临时目录（`.gitignore` 忽略，不要提交）

## 写作规范

- 新文章放在 `source/_posts/`，文件名建议用英文或数字，避免特殊字符
- 文章顶部 Front-matter 必须包含：
  ```yaml
  ---
  title: 文章标题
  date: YYYY-MM-DD HH:mm:ss
  tags: [标签1, 标签2]
  categories: [分类1, 分类2]
  ---
  ```
- 分类/标签不要用重复键，例如不要写两行 `categories:`，应写成数组形式
- 图片放在 `source/images/` 或文章同名资源文件夹中

## 部署说明

- 源码推送到 `main` 分支
- `npx hexo deploy` 会把 `public/` 推送到 `gh-pages` 分支
- GitHub Pages 从 `gh-pages` 分支提供服务
- 不要手动修改 `gh-pages` 分支，应该通过 `hexo deploy` 生成

## 注意事项

- 修改主题配置请编辑 `_config.fluid.yml`，不要直接改 `node_modules/hexo-theme-fluid/` 里的文件
- 运行命令前确保已安装依赖：`npm install`
- Windows 下如果出现 `process_title` 相关报错，通常是终端窗口标题为空导致
