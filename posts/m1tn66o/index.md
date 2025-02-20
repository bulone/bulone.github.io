# 博客“装修”记录


&lt;!--more--&gt;

博客使用的主题是 Fixit，一个简约的主题风格。不过为了满足自己的需求，就用了 deepseek 进行代码生成以及相关问题的解决，极大的帮助了我这半吊子的语言水平。

&gt; [!TIP] 样式修改
&gt; 如果要修改原主题的样式效果，在 `assets` 文件夹中创建 `css/_override.scss` 文件，接着就可以进行样式的修改了。如果效果被覆盖，加个 `!important` 进行强制使用。
&gt; 
&gt; 如果要修改页面的排版布局，将主题里面的 html 文件复制出来在同样的位置即可。

## 链接悬浮效果

原主题对于链接的悬浮效果是变色处理，之前我看到一个博客站的链接是底部从左到右显示横条。于是乎就询问 deepseek，给出的代码方案和预期一致。

链接样式修改后的代码，排除了一些不用该样式的标签。
```scss 普通链接悬浮效果
a:not(.header-title a):not(.featured-image-preview a):not(.TableOfContents a):not(.search a):not(.article-a):not(.lightgallery) {

    position: relative;

    text-decoration: none;

  

    // 默认的悬浮效果（非 .header-title 中的 &lt;a&gt;）

    &amp;:before {

        content: &#34;&#34;;

        position: absolute;

        left: 0;

        bottom: -0.2em;

        width: 0;

        height: 0.2em;

        background-color: $global-color;

  

        [data-theme=&#39;dark&#39;] &amp; {

            background-color: $global-color-dark;

        }

  

        color: transparent;

        transition: width 0.3s ease;

    }

  

    &amp;:hover::before {

        width: 100%;

    }

}
```
主页文章列表悬浮效果 (中间向两边显现)
```scss 
.article-a {

    top: 0;

    left: 0;

    bottom: 0;

    right: 0;

    // position: absolute;

    position: absolute;

    text-decoration: none;

    color: #333;

    display: inline-block;

  

    &amp;::before {

        content: &#34;&#34;;

        position: absolute;

        left: 50%;

        /* 从中间开始 */

        bottom: 0;

        width: 0;

        /* 初始宽度为 0 */

        height: 0.2em;

        background-color: $global-color;

  

        [data-theme=&#39;dark&#39;] &amp; {

            background-color: $global-color-dark;

        }

  

        /* 边框颜色 */

        transition: all 0.3s ease;

        /* 动画效果 */

        transform: translateX(-50%);

        /* 居中 */

    }

  

    &amp;:hover::before {

        width: 100%;

        /* 悬浮时宽度扩展到 100% */

    }

}
```

## 主页

有幸在搜索时进入博主 [Tom Almighty](https://tom2almighty.github.io/hugo-blog-papermod/posts/papermod-modify/) 的博客美化记录，看上了首页图片的排版效果的修改。于是模仿着进行相同效果的修改。

主要修改内容：
- 去除网页 Header 栏的悬浮阴影效果；
- 去除 Header 栏菜单图标的悬浮放大效果
- 修改文章列表格式；
- 去除文章列表分割线；
- 文章列表项目可点击；
- 修改封面图显示效果；

Html 路径 `themes\FixIt\layouts\_default\summary.html`
修改后的首页列表代码，主要新增了一个 flex 效果的 div 包裹封面图片和文章相关信息。
```html
{{- $params := .Params | merge .Site.Params.page -}}

  

&lt;article class=&#34;single summary {{ if $params.featuredimage }} cover-image {{ end }} &#34; itemscope

  itemtype=&#34;http://schema.org/Article&#34;&gt;

  &lt;a class=&#34;article-a&#34; href=&#34;{{ .RelPermalink }}&#34; title=&#39;{{ cond (.Param &#34;capitalizeTitles&#34;) (title .Title) .Title }}&#39;&gt;&lt;/a&gt;

  &lt;div class=&#34;post-entry&#34;&gt;

  

    {{- /* Featured image */ -}}

    {{- $image := $params.featuredimagepreview | default $params.featuredimage -}}

    {{- with .Resources.GetMatch &#34;featured-image&#34; -}}

    {{- $image = .RelPermalink -}}

    {{- end -}}

    {{- with .Resources.GetMatch &#34;featured-image-preview&#34; -}}

    {{- $image = .RelPermalink -}}

    {{- end -}}

    {{- with $image -}}

    &lt;div class=&#34;featured-image-preview&#34;&gt;

      &lt;a href=&#34;{{ $.RelPermalink }}&#34; aria-label=&#34;{{ $.Title }}&#34;&gt;

        {{- dict &#34;Src&#34; . &#34;Title&#34; $.Description &#34;Alt&#34; $.Title &#34;Resources&#34; $.Resources &#34;Width&#34; &#34;50%&#34; &#34;Height&#34; &#34;50%&#34; |

        partial &#34;plugin/image.html&#34; -}}

      &lt;/a&gt;

    &lt;/div&gt;

    {{- end -}}

    &lt;div class=&#34;post-info&#34;&gt;

      {{- /* Title */ -}}

      &lt;h2 class=&#34;single-title&#34; itemprop=&#34;name headline&#34;&gt;

        {{- with $params.weight -}}

        {{- $icon := dict &#34;Class&#34; &#34;fa-solid fa-thumbtack fa-fw&#34; -}}

        &lt;span title=&#34;{{ T &#34; single.pin&#34; }}&#34; class=&#34;icon-pin&#34;&gt;{{- $icon | partial &#34;plugin/icon.html&#34; -}}&lt;/span&gt;

        {{- end -}}

        {{- $repost := $params.repost | default dict -}}

        {{- with $repost -}}

        {{- if eq .Enable true -}}

        {{- $icon := dict &#34;Class&#34; &#34;fa-solid fa-share fa-fw&#34; -}}

        {{- $title := cond (hasPrefix .Url &#34;http&#34;) (printf &#34;%v -&gt; %v&#34; (T &#34;single.repost&#34;) .Url ) (T &#34;single.repost&#34;) -}}

        &lt;span title=&#34;{{ $title }}&#34; class=&#34;icon-repost&#34;&gt;{{- $icon | partial &#34;plugin/icon.html&#34; -}}&lt;/span&gt;

        {{- end -}}

        {{- end -}}

        &lt;a href=&#34;{{ .RelPermalink }}&#34;&gt;{{ cond (.Param &#34;capitalizeTitles&#34;) (title .Title) .Title }}&lt;/a&gt;

      &lt;/h2&gt;

  

      {{- /* Meta */ -}}

      &lt;!-- &lt;div class=&#34;post-meta&#34;&gt;

    {{- partial &#34;single/post-author.html&#34; . -}}

  

    {{- with .PublishDate | dateFormat (.Site.Params.dateFormat | default &#34;2006-01-02&#34;) -}}

      &amp;nbsp;&lt;span class=&#34;post-publish&#34; title=&#39;{{ &#34;2006-01-02 15:04:05&#34; | $.PublishDate.Format }}&#39;&gt;

        {{- printf `&lt;time datetime=&#34;%v&#34;&gt;%v&lt;/time&gt;` . . | dict &#34;Date&#34; | T &#34;single.publishedOnDate&#34; | safeHTML -}}

      &lt;/span&gt;

    {{- end -}}

  

    {{- partial &#34;single/post-included-in.html&#34; . -}}

  &lt;/div&gt; --&gt;

  

      {{- /* Summary content */ -}}

      &lt;div class=&#34;content&#34;&gt;

        {{- if .Summary -}}

        {{- $plainify := (.Param &#34;summaryPlainify&#34;) | default false -}}

        {{- with .Markup &#34;home&#34; -}}

        {{- with .Render -}}

        {{- $summary := dict &#34;Content&#34; .Summary.Text &#34;Ruby&#34; $params.ruby &#34;Fraction&#34; $params.fraction &#34;Fontawesome&#34;

        $params.fontawesome | partial &#34;function/content.html&#34; | safeHTML -}}

        {{- cond $plainify ($summary | plainify) $summary -}}

        {{- end -}}

        {{- end -}}

        {{- else -}}

        {{- .Description | safeHTML -}}

        {{- end -}}

      &lt;/div&gt;

  

      {{- /* Footer */ -}}

      &lt;div class=&#34;post-footer&#34;&gt;

        &lt;div class=&#34;post-meta&#34;&gt;

  

          {{- with .PublishDate | dateFormat (.Site.Params.dateFormat | default &#34;2006-01-02&#34;) -}}

          &lt;span class=&#34;post-publish&#34; title=&#39;{{ &#34;2006-01-02 15:04:05&#34; | $.PublishDate.Format }}&#39;&gt;

            {{- printf `&lt;time datetime=&#34;%v&#34;&gt;%v&lt;/time&gt;` . . | safeHTML -}}

          &lt;/span&gt;

          {{- end -}}

          {{- partial &#34;single/post-included-in.html&#34; . -}}

        &lt;/div&gt;

  

        {{- $tagTerms := .GetTerms &#34;tags&#34; -}}

        {{- if $tagTerms -}}

        &lt;div class=&#34;post-tags&#34;&gt;

          {{- dict &#34;Class&#34; &#34;fa-solid fa-hashtag fa-fw me-1&#34; | partial &#34;plugin/icon.html&#34; -}}

          {{- range $tagTerms -}}

          &lt;a href=&#39;{{ partial &#34;function/escapeurl.html&#34; .RelPermalink }}&#39; class=&#34;post-tag&#34;&gt;{{ .LinkTitle }}&lt;/a&gt;

          {{- end -}}

        &lt;/div&gt;

        {{- end -}}

        &lt;!-- &lt;a href=&#34;{{ .RelPermalink }}&#34;&gt;{{ T &#34;single.readMore&#34; }}&lt;/a&gt; --&gt;

      &lt;/div&gt;

    &lt;/div&gt;

  &lt;/div&gt;

&lt;/article&gt;

{{- /* EOF */ -}}
```
去除分割线
```scss
.home.posts .summary:not(:last-of-type)::after {

    content: &#39;&#39;;

    position: absolute;

    width: 0 !important;

    padding: 0px !important;

}
```
文章首页图片在宽屏状态下放右边小图，窄屏状态下则放上方大图效果。即该文章封面图所呈现的。
```scss
.cover-image {

  

    .featured-image-preview {

        margin: 1rem 1.35rem 1rem 0 !important;

        overflow: hidden;

        display: flex;

        align-items: center;

        height: auto;

        width: 30% !important;

        border-radius: 1rem;

        overflow: hidden;

    }

  

    .post-info {

        width: 70%;

    }

}

  

.post-entry {

    display: flex;

    flex-direction: column;

    height: 100%;

    flex-direction: row-reverse;

}

  

.post-info {

    padding: 1.35rem;

    width: 100%;

}

  

@media (max-width: 680px) {

    .cover-image {

        .post-entry {

            flex-direction: column;

        }

s

        .featured-image-preview,

        .post-info {

            width: 100% !important;

            box-sizing: border-box;

        }

  

        .featured-image-preview {

            margin: 0 !important;

            border-radius: 1rem 1rem 0 0;

        }

    }

}

  

.featured-image,.lightgallery {

    img {

        border-radius: 1rem !important;

    }

}

.post-entry .post-footer {

    margin-block: 0.4rem 0;

    display: flex;

    justify-content: space-between;

    align-items: center;

    font-size: 0.875rem;

    flex-wrap: wrap;

  

    /* 允许内容换行 */

    .post-meta,

    .post-info {

        flex: 1;

        /* 让子元素平分空间 */

        min-width: 50%;

        /* 设置最小宽度，防止内容过小时换行 */

    }

}
```

## 文章页
- 修改图片显示效果
主要是不在链接后面加上标题图片就不居中显示，在配置文档中添加 Force 也没用，干脆就直接修改原页面代码内容，其实就是复制粘贴。
html： `\themes\FixIt\layouts\_default\_markup\render-image.html`
```html
{{- /* TODO refactor */ -}}

{{- $params := .Page.Params | merge site.Params.page -}}

{{- if .Title | and .Text -}}

{{- $linked := ne $params.lightgallery false -}}

&lt;figure&gt;

  {{- dict &#34;Src&#34; .Destination &#34;Alt&#34; .Text &#34;Caption&#34; .Text &#34;Title&#34; .Title &#34;Linked&#34; $linked &#34;Responsive&#34; true &#34;Resources&#34;

  .Page.Resources | partial &#34;plugin/image.html&#34; -}}

  &lt;figcaption class=&#34;image-caption&#34;&gt;

    {{- .Text | safeHTML -}}

  &lt;/figcaption&gt;

&lt;/figure&gt;

{{- else -}}

{{- $linked := (eq $params.lightgallery &#34;force&#34;) | or ($params.lightgallery | and (ne .Title &#34;&#34;)) -}}

  

&lt;figure&gt;

  {{- dict &#34;Src&#34; .Destination &#34;Alt&#34; .Text &#34;Title&#34; .Title &#34;Linked&#34; $linked &#34;Responsive&#34; true &#34;Resources&#34; .Page.Resources

  | partial &#34;plugin/image.html&#34; -}}

  &lt;figcaption class=&#34;image-caption&#34;&gt;

    {{- .Text | safeHTML -}}

  &lt;/figcaption&gt;

&lt;/figure&gt;

{{- end -}}
```
## 字体

十分感谢❤[中文网字计划](https://chinese-font.netlify.app/zh-cn/cdn/)，本站的网站 title 字体和作者姓名都是使用了该网站的字体 `全小素` 的 CDN，其他字体除代码字体外使用的是 `OPPO Sans 4.0 SC`。也是使用了该网站的分包技术，是的网站依旧能快速加载。

```SCSS
@import url(&#34;你的字体链接&#34;);

//如若修改全局字体示例（原主题的变量命名）
$global-font-family: &#34;OPPO Sans 4.0 SC&#34;, &#34; QuanPixel 8px&#34;, system-ui, -apple-system, BlinkMacSystemFont, PingFang SC, Microsoft YaHei UI, Segoe UI, Roboto, Oxygen, Ubuntu, Cantarell, Fira Sans, Droid Sans, Helvetica Neue, Helvetica, Arial, sans-serif;
```

目前改的内容好像也就这么多，如果后续有新内容修改，继续更新内容吧。

---

> 作者: 蓝莓吐司  
> URL: http://localhost:1313/posts/m1tn66o/  

