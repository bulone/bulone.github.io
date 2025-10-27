# Fixit主题美化


<!--more-->
==本文章代码部分内容使用 AI 辅助创作==[danger]

安装博客也一月有余，期间也拜访过其他博客站，当然是因为搜索知识进行拜访。对于主题的一些内容进行自定义并完善。

**注：涉及到 html 修改部分请将所需修改文件复制一份到主题外相同位置，修改后既能应用也能不破坏原主题文件。**

## CSS 样式
对于主题 CSS 参数，可以在 `assets/css` 中创建 `_override.scss` 进行设置，会进行覆盖。  其他样式可以在同文件夹内创建 `_custom.scss` 进行编写。  

本博客使用的是个人免费使用的 [OPPO Sans4.0](https://www.coloros.com/article/A00000074/) 字体，主要喜欢这个字体的数字部分。利用 [中文网字计划网站](https://chinese-font.netlify.app/zh-cn/) 进行字体分包，在使用字体后不影响网站访问速度。
## 首页列表部分

原主题的首页列表部分是每行一个，列排布。没有封面图还好，如果有封面图因为比例显示为 2:1，所以在屏幕上会显得很大，那么一页下来也显示不了几张图片。偶然进入 [Atpx](https://atpx.com/blog/) 的空间，发现其博客页很简约整齐，双列排布文章，每个文章块也很小。所以就借鉴了其样式内容，为了不同宽度设备整个块比例适当，所以也顺势调整了图片的宽高比达到合适程度。  

文件位置：
- layouts/\_default/home.html & summary.html 
- layouts/partials/single/post-included-in.html

![文章列表图](./images/index-1742907969794.webp  "文章列表图")
## 文章页
主要删除了作者信息，其余未作变化。  
文件位置：layouts/post/single.html

![文章页顶部](./images/index-1742908689762.webp "调整位置")

## MSEmoji🎉

![项目宣传图](./images/index-1742908925710.webp  "表情展示")  
主题自带有 Twemoji，可以在主题配置文件中选择开启。不过我开启后还是加载失败，所以还是把他关闭了。很喜欢微软的表情，在 Github 上搜索类似 Twemoji 的项目，最终找寻该项目符合。  

{{< link "https://github.com/DellZHackintosh/msemoji"  "Msemoji" " 基于 Twemoji 修改的微软风格表情 Emoji。"   true >}}

**使用方法**：
1. 在配置文档中搜索 `params.page.library.js`，在其内部添加  
   msemojijs = "https://unpkg.com/msemoji@1.1.2/dist/msemoji.min.js"
2. 创建 `custom.js`，位置在 `assets/js/`
```js
msemoji.parse(document.body,{
    base: "https://gcore.jsdelivr.net/gh/DellZHackintosh/msemoji@1.1.2/src/",
    ext: ".svg", 
    folder: "Color"
  });
```

| base   | ext   | folder |
| ------ | ----- | ------ |
| 表情仓库位置 | 表情拓展名 | 表情所在目录 |

主要这三个参数即可，如果还想要其他参数配置，请参考原文档。

## Waline 评论系统
博客使用的是 Waline，因为就它带有文章 Reaction，可以自定义图片。其他的几个评论系统都大差不差。使用方法请到 [Waline官网](https://waline.js.org/) 配置参数。

## AOS 动画库

滑动动画库，依赖于 Jquery。  
在 `hugo.toml` 中的 [params.page.library] 加入所需 CSS，Js 文件。然后再 _custom.js 中添加
```js
AOS.init({
	once:true,
	duration: 400,
	delay: 0,
});
```
init 内可以自选参数初始化。
## 相关链接

1. 文章 Reaction 表情：[Animated-Fluent-Emojis](https://github.com/Tarikul-Islam-Anik/Animated-Fluent-Emojis)
2. 辅助生成样式代码：[Deepseek](https://www.deepseek.com)
3. AOS动画库使用：[前端炫酷的动画效果库--AOS使用详解\_aos-easing-CSDN博客](https://blog.csdn.net/weixin_45277161/article/details/112302569)
4. AOS动画库插件：[插件 ｜ AOS 滚动动画库aos.js - 掘金](https://juejin.cn/post/7018850667653496839)

---

> 作者: 吐司气泡  
> URL: https://blog.toastbubble.top/posts/aa6eb19/  

