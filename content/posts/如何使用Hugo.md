---
title: "如何使用Hugo"
date: 2023-05-30T16:11:22+08:00
draft: false
---

### 安装hugo
首先，去Hugo官网https://gohugo.io/下载适合您操作系统的最新版本。

brew install hugo

hugo version 


然后，将Hugo解压缩到您想要的位置。例如，您可以将其解压缩到/usr/local/bin/，这将使Hugo可用于整个系统。

接下来，打开命令行终端并运行以下命令来检查Hugo是否已成功安装：
```bash
hugo version
```

如果一切正常，您应该会看到Hugo的版本信息。

### 新建站点
```bash
hugo new site /path/to
```
### 新建博文
hugo new posts/my-first-post.md

生成您的网站静态文件。在命令行中输入以下命令：
```bash
hugo
```
这将使用Hugo生成静态网页，并将它们存储在“public”目录中。

此时的目录
```bash
.
├── archetypes
│   └── default.md
├── assets
├── config.toml
├── content
│   └── 如何使用Hugo.md
├── data
├── layouts
├── public
│   ├── categories
│   │   └── index.xml
│   ├── index.xml
│   ├── sitemap.xml
│   └── tags
│       └── index.xml
├── resources
│   └── _gen
│       ├── assets
│       └── images
├── static
└── themes
```


## 配置github仓库

配置域名

## 配置CI/CD

github 选择Action 新建Action 然后搜索hugo，会得到一个
```# Sample workflow for building and deploying a Hugo site to GitHub Pages```
然后保存

新建分支 github-pages

设置github-pages为主分支

## 配置域名
Configuring a custom domain for your GitHub Pages site
[github docs](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site)


### 常见问题

#### failed with exit code 128
> The process '/usr/bin/git' failed with exit code 128.

[参考](https://blog.csdn.net/nxg0916/article/details/129063959)

1. 创建个人访问令牌
https://docs.github.com/zh/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token

使用 GitHub API 或命令行时，可使用 Personal access token 替代密码向 GitHub 进行身份验证。
 
创建完成后，复制Token

2. 返回仓库创建 Actions secrets and variables

3. 修改yaml里面的token
```bash
github_token: ${{ secrets.ACCESS_TOKEN }}
```

4. action 执行完毕

