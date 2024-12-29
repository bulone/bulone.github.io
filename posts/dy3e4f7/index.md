# Hugo&#43;Obsidian自动部署


&lt;!--more--&gt;

之前搭建的博客站都是基于 php 构建的博客程序，对于备份来说是有点麻烦。也尝试过静态博客，觉得写文章和发布文章时比有 UI 界面的麻烦了很多。但是了解了 Obsidian 和 GitHub Page 后，创建文章也很简单。**以下为我个人配置，不代表大多数人**。

## Hugo

在 Windows 上通过 Scoop 管理软件进行 Hugo 安装。
1. 打开 PowerShell 粘贴以下代码安装 Scoop
```
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
Invoke-RestMethod -Uri https://get.scoop.sh | Invoke-Expression
```
2. 然后通过 scoop install hugo-extended 安装 hugo extended 版本。
3. hugo new site 路径名 生成 hugo 结构。
4. 将该结构创建 Git 上传到 Github 库中。

## Obsidian

1.将上述 Hugo 结构目录作为 Obsidian 的文档库，由于 Obsidian 会默认生成.obsidian 文件夹已保存相关配置，所以要在.gitignore 中将.obsidian/ 进行忽略防止上传。
2.设置中新建文章位置指定为 content/posts/下。
3.使用模板或者 quickadd 对于新文档的创建，减少和在进入命令行进行生成文档。
4.写完文章后可以通过 git 插件或者通过 git 命令进行更新。

## Github WorkFlow
1.在 [Personal access tokens (classic)](https://github.com/settings/tokens) 生成一个新 token，根据个人需求选择过期时间或不过期。切记，只会在创建后显示一次，因此要保存好 Token。当然忘记后创建新的也是可以的。
2.在 GitHub 库的设置中选择 Secrets or variables - Actions 创建一个名为 ACTIONS_DEPLOY_KEY，将上述 token 粘贴到框内保存。
3.创建一个 GitHub Action，将下述代码复制粘贴，再根据文字内容进行修改。
```yml
name: github_pages # 名字自取

on:
  push:
    branches:
      - master  # 这里的意思是当 main分支发生push的时候，运行下面的jobs，这里先改为github-actions

jobs:
  deploy: # 任务名自取
    runs-on: ubuntu-latest	# 在什么环境运行任务
    steps:
      - uses: actions/checkout@v2	# 引用actions/checkout这个action，与所在的github仓库同名
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive) 获取submodule主题
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo	# 步骤名自取
        uses: peaceiris/actions-hugo@v2	# hugo官方提供的action，用于在任务环境中获取hugo
        with:
          hugo-version: &#39;latest&#39;	# 获取最新版本的hugo
          # extended: true

      - name: Build
        run: hugo --minify	# 使用hugo构建静态网页

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3	# 一个自动发布github pages的action
        with:
          # github_token: ${{ secrets.GITHUB_TOKEN }} 该项适用于发布到源码相同repo的情况，不能用于发布到其他repo
          external_repository: 	# 发布到哪个repo
          personal_token: ${{ secrets.ACTIONS_DEPLOY_KEY }}	# 发布到其他repo需要提供上面生成的personal access token
          publish_dir: ./public	# 注意这里指的是要发布哪个文件夹的内容，而不是指发布到目的仓库的什么位置，因为hugo默认生成静态网页到public文件夹，所以这里发布public文件夹里的内容
          publish_branch: main	# 发布到哪个branch
```
4. 之后当 GitHub 库更新后就会自动重新发布 Github Page。

## 问题

1. 因为 Obsidian 和 Hugo 的图片链接不同，导致其中一个显示另一个就不显示。
	[# [Help with link images to see from absolute path including “/static”?](https://discourse.gohugo.io/t/help-with-link-images-to-see-from-absolute-path-including-static/41177)](https://discourse.gohugo.io/t/help-with-link-images-to-see-from-absolute-path-including-static/41177/3)

# Obsidian 插件



![插件](/images/Hugo&#43;Obsidian自动部署-20241229171358468.webp)

---

> 作者: bulone  
> URL: http://blog.hifo.fun/posts/dy3e4f7/  

