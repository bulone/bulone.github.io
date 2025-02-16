# 建站日志


&lt;!--more--&gt;

建站四部走：
- 安装框架
- 选择主题
- 推送仓库
- 创建 Github Page
## 安装框架

静态博客网站不少，热门的也多。不过挑来挑去还是 Hugo 适合，尽管对应的主题相对于 Hexo 来说少的可怜，但是还是有非常不错的主题选择。

Hugo 的安装官网上有十分详细的介绍，而且都简单易懂，相关命令行也都是直接给予，复制粘贴即可。由于本人是 Windows 系统，所以利用 Scoop 进行安装和后续的 Hugo 版本升级，都是一句命令的事情。
### 安装 Scoop
Open a PowerShell terminal (version 5.1 or later) and from the PS C:\&gt; prompt, run:(非管理员模式)

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
Invoke-RestMethod -Uri https://get.scoop.sh | Invoke-Expression
```

### 安装框架
1. 运行下述命令等待安装完成
```
scoop install hugo-extended
```
2. 创建网站(选择合适位置)
```
hugo new site xxx
```
## 选择主题
当然还是[Fixit](https://github.com/hugo-fixit/FixIt)，主要是Fixit的教程十分详细，而且还是国内开发者，对于一些东西都进行了详细的优化，因此不二之选（记得在Github上给开发者一个⭐Star）。

详细教程请到[开发者主题网站](https://fixit.lruihao.cn/zh-cn/)根据教程来配置

## 推送仓库
利用Github Page创建博客网站，将生成的public传送到Github Page仓库或者将博客框架主体传送到新建的仓库，再利用Workflow进行推送也行。不过还是第二种可靠些，毕竟将主体都备份到Github上。以后换电脑时可以直接克隆仓库。

1. 创建主体仓库
2. 创建GIthub Page仓库
3. 每次更新直接推送主体仓库

&gt; workflow代码
```
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
          external_repository: bulone/bulone.github.io	# 发布到哪个repo
          personal_token: ${{ secrets.ACTIONS_DEPLOY_KEY }}	# 发布到其他repo需要提供上面生成的personal access token
          publish_dir: ./public	# 注意这里指的是要发布哪个文件夹的内容，而不是指发布到目的仓库的什么位置，因为hugo默认生成静态网页到public文件夹，所以这里发布public文件夹里的内容
          publish_branch: main	# 发布到哪个branch
```

---

> 作者: 蓝莓吐司  
> URL: http://localhost:1313/posts/jbb6623/  

