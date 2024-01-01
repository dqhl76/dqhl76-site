---
title: 使用 Github Action 实现 Hexo 的自动部署
date: 2023-08-10 14:01:12
tags: 
- Github
- Hexo
- Github Action
- Github Pages
---

据说每一个写代码的人都有一个博客梦，不管是平时学习的内容，还是一些日常的感受，都可以分享出来。

作为我在更换为 Hexo 后的第一篇文章，就讲讲如何使用 Github Action 来自动将你托管在 Github 的上 Hexo 项目部署到 Github Page 上。

本文更偏向于面向一个{% wavy 没有相关经验的朋友 %}分享如何实现，如果你已经有相关经验，可以直接复制走配置文件。

## 下载并初始化Hexo

安装 Hexo 需要你首先具备 Nodejs 的环境，如果你没有安装 Nodejs，可以在官网选择对应的系统版本进行下载安装。
{% copy https://nodejs.org/en/download/ %}


如果你是用 Homebrew 进行包管理，简单运行：

```bash
brew install node
```

安装完成 node 后，运行以下命令安装 Hexo：

```bash
npm install -g hexo-cli
```

安装完成后，运行以下命令初始化 Hexo：

```bash
hexo init <folder>
cd <folder>
npm install
```

## 配置 Hexo

修改 Hexo 的配置文件 `_config.yml`，将 `title` `subtitle` `author` 等条目按照你的需要进行配置。

这里{% emp 需要注意 %}的是，如果你的需要部署到 Github Page，那么你的域名就是 `username.github.io/<repo>`，那么你需要将 `url` 这个条目配置为 `https://<username>.github.io/<repo>`，否则你的博客网站将缺乏样式文件而无法正常渲染。

同理如果你想要配置为自己的域名，也需要在这里进行配置，拿我的举例则为：`https://blog.realdqhl.com`

## 常用的 Hexo 指令

创建一篇新的文章：
```bash
hexo new <title>
```

生成静态文件：
```bash
hexo generate
```

启动本地服务：
```bash
hexo server
```

## 配置 Github Action

在你的 Hexo 项目中，创建一个 `.github/workflows` 的文件夹，然后在里面创建一个 `pages.yml` 的文件，这个文件就是我们的 workflow 配置文件。

```yml
name: Pages

on:
  push:
    branches:
      - main  # default branch
  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:
  
# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write
  
jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: actions/checkout@v3
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          submodules: recursive
      - name: Use Node.js 16.x
        uses: actions/setup-node@v2
        with:
          node-version: '16'
      - name: Cache NPM dependencies
        uses: actions/cache@v2
        with:
          path: node_modules
          key: ${{ runner.OS }}-npm-cache
          restore-keys: |
            ${{ runner.OS }}-npm-cache
      - name: Install Dependencies
        run: npm install
      - name: Build
        run: npm run build
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v2
        with:
          path: ./public
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2

```

看了一下，这里和一些教程可能不太一样，比如说没有使用将静态文件推送到 `gh-pages` 分支的方式，而是使用了 `actions/upload-pages-artifact@v2` 这个 action 来将静态文件上传到服务器上，然后再使用 `actions/deploy-pages@v2` 这个 action 来将静态文件推送到 Github Page 上。

随后你需要创建一个 GitHub 项目仓库，然后将你本地的文件上传。上传后，需要在项目的 setting 中进行配置，这里我们将 `Build and deployment -> Source` 选择为 `GitHub Actions`，并根据需要配置你自己的域名就可以了。

![](https://github.com/dqhl76/dqhl76-site/assets/69136320/31b74b2c-71f1-4e12-8be7-6e113e124770)

