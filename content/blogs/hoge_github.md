---
title: "Hugo和GitHub搭建博客"
date: '2025-12-17T12:31:27+08:00'
draft: false
github_link: "https://turing-dz.github.io/"
author: "Zoe"
tags:
  - life
image: /images/blogs/blog.jpeg
description: "博客部署教程"
toc: 
---
本文主要是使用hugo和github进行个人博客网站的搭建，21年刚上班的时候搭过一版，没怎么用，希望这次能坚持在这里记录自己的博客。

# 1.本地hugo安装
因为有版本兼容问题，所以先去选择主题，然后看看主题对应推荐的hugo版本，然后下载对应版本的hugo。
[hugo主题社区](https://themes.gohugo.io/)里面选择，我选择了[hugo-profile](https://hugo-profile.netlify.app/)。
```bash
brew install hugo@0.145.0           #下载安装，如果失败，就下载这个版本的tar.gz文件，解压，然后sudo mv hugo /usr/local/bin/hugo把解压后的hugo放置到path目录下
sudo xattr -rd com.apple.quarantine /usr/local/bin/hugo     #开发者验证
hugo version             #查看版本
hugo new site my-blog           #创建博客
cd my-blog
git init
```
然后修改主题里面的内容

```bash
git submodule add https://github.com/gurusabarish/hugo-profile.git themes/hugo-profile #下载theme主题
cp -f themes/hugo-profile/exampleSite/hugo.yaml ./hugo.yaml     #复制出来配置文件
rsync -av themes/hugo-profile/exampleSite/static/ ./static/         #复制出来静态文件
rsync -av themes/hugo-profile/exampleSite/content/ ./content/        #复制出来内容文件
```
启动

```bash
hugo server -D
```

[http://localhost:1313](http://localhost:1313)打开预览。

`hugo.yaml`文件修改首页里面的内容。
`content/blogs`里面写博客。
`static/images`里面放置图片文件。


# 2.部署到 GitHub Pages

1.github新建仓库Public ，不要勾 README（可有可无）
```bash
GitHub用户名.github.io
```
2.本地项目关联 GitHub
```bash
git add .
git commit -m "init hugo blog"
git branch -M main
git remote add origin https://github.com/Turing-dz/Turing-dz.github.io.git
打开 GitHub → 右上角头像 → Settings → Developer settings →Personal access tokens → Tokens (classic) →点击 Generate new token (classic) →勾选：repo，workflow
git push -u origin main
```
3.添加 GitHub Actions（自动构建 Hugo）

```bash
mkdir -p .github/workflows
touch .github/workflows/hugo.yml
```
把下面内容 完整复制 到 hugo.yml

```yml
name: Deploy Hugo site to GitHub Pages

on:
  push:
    branches:
      - main

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
          submodules: true
          fetch-depth: 0

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v3
        with:
          hugo-version: '0.145.0'
          extended: true

      - name: Build
        run: hugo --minify

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
并更新到仓库

```
git add .github/workflows
git commit -m "fix pages workflow"
git push
```

4.仓库里配置 GitHub Pages
在 GitHub 仓库里，Settings → Pages，Source: GitHub Actions
5.修改 Hugo 配置
打开本地hugo.yaml，修改baseurl为自己的地址。

```bash
baseURL: "https://turing-dz.github.io/"
```
然后提交仓库

```bash
git add hugo.yaml
git commit -m "set baseURL"
git push
```
进入仓库查看部署状态并访问即可。
# 3.写博客
```bash
# 写文章
vim content/blogs/xxx.md

# 本地看
hugo server

# 发布
git add .
git commit -m "new post"
git push
```


