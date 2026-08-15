---
title: "用 Jekyll 搭建自己的 GitHub 个人主页"
date: 2026-08-15 10:30:00 +0800
excerpt: "从 Ruby + Codex 到 GitHub Pages 与常见构建错误记录。"
categories:
  - building
tags:
  - jekyll
  - github-pages
  - ruby
  - scss
published: true
permalink: /blog/building/jekyll-github-pages/
---

GitHub Pages 适合托管不需要后端的个人主页：内容保存在 Git 仓库，推送后由 GitHub 构建并发布。本篇以一个使用 [Minimal Mistakes](https://github.com/mmistakes/minimal-mistakes) 远程主题的 Jekyll 站点为例，整理从本地环境到部署、再到样式定制的完整流程。

<!--more-->

### 珠玉在前

直接利用 GitHub Pages 或者 clone 别人的仓库搭建主页，是一种很方便的方式，有许多博客珠玉在前：
- [使用github.io制作你的个人学术主页-知乎](https://zhuanlan.zhihu.com/p/711554540)
- [如何用Github Pages搭建自己的个人网站？- Qiyu Chen](https://classicalqy.github.io/website_building/)

此处记录的则是我先用 Codex + Ruby 在本地搭建 jekyll 框架，再通过 `git remote add` 连接远程仓库，再推送上去得到个人主页的步骤。我的 prompt:

> i want to build a personal homepage based on Jekyll(Github) minimal mistake, academic style, including main homepage(About Me + Research Intrest + Publications + Projects + one picture of me), blog(which i can set as private or public and update), friend-link. Can you help me build my homepage under 'homepage' workspace?  I know i should create a repo on my github and push my homepage, so you can just help me clone and build and edit and beautify locally, i'll commit and push by myself.


### 推荐版本

一个 Jekyll 主页通常有三层依赖，分别由不同工具处理：

1. **运行时**：Ruby、RubyGems 和 Bundler 在本地运行 Jekyll。
2. **站点依赖**：`Gemfile` 中的 gem，例如 `github-pages`、`webrick`。`vendor/bundle` 是 Bundler 在本地缓存 Ruby gem 的目录；它不会替 GitHub Pages 下载远程主题，也不应提交到 Git 仓库。如果搭建主页时本地用了 `vendor/bundle`，GitHub Action 很可能会报错并推荐你改为远程主题。
3. **主题与配置**：`_config.yml` 选择主题、插件和 URL；CSS 和 SCSS 则负责覆盖主题的视觉样式。

本仓库建议使用 **Ruby 3.3.x**，这是为了让本地环境与本项目的 GitHub Pages 依赖组合保持稳定，防止更高的版本不支持 csv， 并不是所有 Jekyll 项目的通用上限。我先后尝试了 Ruby 4.0 -> 3.4 -> 3.3，认为 **Ruby 3.3** 是最好的。

Windows 用户推荐安装带 MSYS2 DevKit 的 [RubyInstaller](https://rubyinstaller.org/)。DevKit 能在某些 gem 需要本地编译扩展时提供编译工具。Installer 提供了便捷安装和 PATH 配置，下载好之后双击启动 Ruby 的安装，安装时一路 Next 即可。也可以通过命令行安装：

```powershell
winget install --exact --id RubyInstallerTeam.RubyWithDevKit.3.3 --source winget --accept-package-agreements --accept-source-agreements
```

安装后重启终端，确认版本：

```powershell
ruby --version
gem --version
```

有正常输出代表已经安装完毕，且 PATH 配置好。

### Gemfile 应该包含什么

一个使用 GitHub Pages 和远程主题的最小 `Gemfile` 可以是：

```ruby
source "https://rubygems.org"

gem "github-pages", group: :jekyll_plugins
gem "webrick", "~> 1.8"
```

`github-pages` 让本地 Jekyll 依赖尽量贴近 GitHub Pages 的构建环境，并带来 Pages 支持的插件。`webrick` 只用于本地启动开发服务器。主题的远程下载由 `jekyll-remote-theme` 处理，它应列在 `_config.yml` 的 `plugins` 中。

如果使用 `remote_theme`，不要同时为了同一个主题再添加 `minimal-mistakes-jekyll` 这类本地主题 gem。两种安装模式混用容易让 Bundler 的约束与 GitHub Pages 的预装依赖发生冲突。选择一种模式并保持配置一致。

## 从零到本地预览

现在，假定我们已经安装好 Ruby 和 gem，并且本地拥有一个包含 `Gemfile` `index.md` `_config.yml` 等必要结构的 jekyll 克隆仓库或手动仓库（我是直接让 Codex 拉取并搭建的框架）。在仓库目录下打开命令行安装 bundler：

```powershell
gem install bundler
bundle --version
bundle install
bundle exec jekyll serve
```
这里的最后一行命令就是开启本地预览，打开 `http://localhost:4000` 查看网站。`--livereload` 会在保存 Markdown、YAML 或 SCSS 后自动刷新浏览器；如果它在本机不可用，去掉该参数即可。`--trace` 会追踪网页日志。

## 配置 GitHub Pages 与远程主题

对于用户主页仓库，仓库名应为 `USERNAME.github.io`。在 `_config.yml` 中设置站点 URL、仓库名和主题：

```yaml
url: "https://USERNAME.github.io"
baseurl: ""
repository: "USERNAME/USERNAME.github.io"

remote_theme: "mmistakes/minimal-mistakes@4.24.0"

plugins:
  - jekyll-remote-theme
  - jekyll-feed
  - jekyll-include-cache
  - jekyll-sitemap
```

远程主题中的版本必须是上游真实存在的 Git tag。Minimal Mistakes 的这个标签写作 `4.24.0`，而不是 `v4.24.0`。标签写错时，GitHub Pages 会在下载 `codeload.github.com` 的主题压缩包时返回 404，导致整个构建失败。另外，GitHub Action 不接受本地主题（即使 vendor 预览成功），Action 会自动远程拉取主题，所以上述 plugins 和 `remote_theme` 是不可或缺的。

在 GitHub 仓库的 `Settings -> Pages` 中，选择 `Deploy from a branch`，再选择 `main` 或其他分支和 `/(root)`。这项设置只需配置一次；以后每次推送到该分支都会触发重新发布。也可以改用 GitHub Actions，但那意味着要自行维护构建工作流和依赖版本。[GitHub Pages 发布源文档](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site) 说明了两种方式的区别。


## 提交与发布

本地预览确认无误后，提交并推送到 Pages 的发布分支：

```powershell
git remote add https://github.com/[username]/[username].github.io.git
git status
git add xxx
git commit -m "xxx"
git push origin [your deploy-page branch]
```
如果还改了样式或站点配置，再有针对性地把 `assets/css/main.scss`、`_config.yml` 或 `Gemfile` 加入暂存区。

推送后在仓库的 `Actions` 页面查看部署状态。构建完成后，访客访问的就是发布分支中 `/(root)` 的最新版本。首次发布或依赖下载较慢时，等待几分钟再刷新页面。

## 常见问题与排查

| 现象 | 原因 | 处理方式 |
| --- | --- | --- |
| `bundle` 不是命令 | Ruby 或 Bundler 没有安装，或终端没有刷新 `PATH` | 重开终端，执行 `ruby --version`，再运行 `gem install bundler`。 |
| `Bundler can't satisfy your Gemfile's dependencies` | 本地主题 gem、远程主题和 `github-pages` 的版本约束混在一起 | 选择一种主题模式。远程主题项目保留 `github-pages` 和 `_config.yml` 中的 `remote_theme`，不要再为同一主题安装本地 gem。 |
| `404 ... codeload.github.com/.../zip/v4.24.0` | `remote_theme` 的 tag 不存在 | 到主题仓库的 Releases 或 Tags 页面核对标签；此例应写成 `@4.24.0`。 |
| `vendor/bundle` 已存在但 Pages 仍报主题下载错误 | `vendor/bundle` 只在本机缓存 Bundler 安装的 gem；GitHub Pages 使用自己的干净构建环境 | 不提交 `vendor/`。检查远程主题地址、tag 和 `_config.yml` 插件列表。 |
| `Invalid username or token` | GitHub 已不支持用账号密码进行 HTTPS Git 推送，或旧令牌被缓存 | 使用 Git Credential Manager 浏览器授权，或使用有仓库写入权限的 Personal Access Token。 |
| `Failed to connect to github.com port 443` | 网络、防火墙、代理或 VPN 无法连接 GitHub | 先恢复 HTTPS 连通性，再重试推送；认证错误和网络错误要分别处理。 |

一个稳定的工作流很简单：内容和样式在本地修改，Jekyll 预览，Git 提交，推送到发布分支，再由 GitHub Pages 自动部署。把依赖模式保持单一、把本地生成目录排除在版本控制外，就能少掉大部分环境问题。
