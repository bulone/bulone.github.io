# 跳转中间页


&lt;!--more--&gt;
## 引言
之前挺烦一些网站的链接跳转，类如 CSDN，掘金之类的，浪费时间还浪费精力。不过对于一些链接做中间页跳转还是有必要的，毕竟一些网站域名过期被一些非法网站使用，那么就会导致博客内文章直接引向非法网站，这是万万不可的。所以做中间页提醒访客链接安全未知，请保护好个人信息。相关方面参考了一些网站内容，🙏感谢这些博主的分享。  
![跳转页效果](./images/index-1742980281921.webp &#34;效果展示&#34;)
## 创建

&gt; [!CAUTION] 声明  
&gt; 跳转页静态代码和 CSS 样式在 DeepSeek 生成的基础上进行修改，使用了 AI 辅助创作。

1. 复制一份主题文件中的 layouts/_default/_markup/render-link.html 到根目录，并修改成如下。
```html
{{- $destination := .Destination -}}
{{- $isExternal := not (hasPrefix $destination &#34;/&#34;) -}}
{{- with dict &#34;Path&#34; $destination | partial &#34;function/resource.html&#34; -}}
  {{- $destination = .RelPermalink -}}
  {{- $isExternal = false -}}
{{- end -}}

{{- if $isExternal -}}
  {{/* 外部链接 - 添加拦截处理 */}}
  &lt;a href=&#34;/redirect/?url={{ $destination | base64Encode }}&#34;
     class=&#34;external-link&#34;
     data-external-destination=&#34;{{ $destination }}&#34;
     title=&#34;{{ .Title }}&#34;
     target=&#34;_blank&#34;
     {{ if $.Page.Site.Params.externalIcon }}data-external-icon=&#34;true&#34;{{ end }}&gt;
    {{ .Text }}
    {{ if $.Page.Site.Params.externalIcon }}&lt;span class=&#34;external-icon&#34;&gt;↗&lt;/span&gt;{{ end }}
  &lt;/a&gt;
{{- else -}}
  {{/* 内部链接 - 正常渲染 */}}
  {{- $options := dict &#34;Destination&#34; $destination &#34;Title&#34; .Title &#34;Content&#34; .Text -}}
  {{- partial &#34;plugin/link.html&#34; $options -}}
{{- end -}}

```
2. 在 `layouts/_default`/中创建 `redirect.html` 文件，复制下面代码。  
适用于 Fixit 主题，其他主题自行测试。
```html
{{- define &#34;title&#34; -}}
  {{- cond (.Param &#34;capitalizeTitles&#34;) (title .Title) .Title -}}
  {{- if .Site.Params.withSiteTitle }} {{ .Site.Params.titleDelimiter }} {{ .Site.Title }}{{- end -}}
{{- end -}}

{{ define &#34;custom-head&#34; }}
    &lt;meta name=&#34;robots&#34; content=&#34;noindex, nofollow&#34;&gt;
    &lt;meta name=&#34;referrer&#34; content=&#34;no-referrer&#34;&gt;
    &lt;meta charset=&#34;utf-8&#34;&gt;
    &lt;meta name=&#34;viewport&#34; content=&#34;width=device-width, initial-scale=1&#34;&gt;
{{ end }}

{{ define &#34;content&#34; }}
    &lt;div class=&#34;external-warning&#34;&gt;
        &lt;h2 class=&#34;warning-title&#34;&gt;⚠️ 您即将离开 {{ .Site.Title }}&lt;/h2&gt;
        &lt;p&gt;将要访问的外部链接：&lt;/p&gt;
        &lt;div class=&#34;external-url&#34; id=&#34;target-url&#34;&gt;{{ .Params.url }}&lt;/div&gt;
        
        &lt;p&gt;🛡请自行识别链接是否安全，注意您的帐号和财产安全。&lt;/p&gt;
        
        &lt;div class=&#34;button-group&#34;&gt;
            &lt;a href=&#34;{{ .Params.url }}&#34; class=&#34;button continue-btn&#34; id=&#34;proceed-link&#34;&gt;🚀 继续访问&lt;/a&gt;
        &lt;/div&gt;
    &lt;/div&gt;

    &lt;script&gt;
        // 解码URL并显示
        const params = new URLSearchParams(window.location.search);
        const encodedUrl = params.get(&#34;url&#34;);
        const decodedUrl = atob(encodedUrl);
        document.getElementById(&#39;target-url&#39;).textContent = decodedUrl;
        document.getElementById(&#39;proceed-link&#39;).href = decodedUrl;
    &lt;/script&gt;
{{ end }}
```

3. 在 content 中创建 `redirect.md`。
```yaml
---
title: 风险跳转提示
layout: &#34;redirect&#34;
type: &#34;redirect&#34;
---
```
title 指的是跳转页网站 Title，layout 指的是使用 redirect 模板。

4. 在 `custom.scss` 中粘贴下述样式。
```scss
/* -------------------- 跳转页 ------------------- */
.external-warning {
    max-width: 700px;
    margin: 3rem auto;
    padding: 2rem;
    border: 1px solid #eee;
    border-radius: 1rem;
    box-shadow: 0 2px 12px rgba(0,0,0,0.1);
    line-height: 1.6;
    text-align: center;
}
.warning-title {
    color: #d32f2f;
    margin-top: 0;
}
.external-url {
    word-break: break-all;
    background: #f5f5f5;
    [data-theme=&#34;dark&#34;] &amp; {
	    background: #363636
    }
    padding: 0.5rem;
    border-radius: 1rem;
    display: inline-block;
    max-width: 100%;
    overflow: hidden;
    text-overflow: ellipsis;
}
.button-group {
    margin-top: 1.5rem;
    display: flex;
    gap: 1rem;
    flex-wrap: wrap;
    justify-content: center;
}
.button {
    padding: 0.75rem 1.5rem;
    border-radius: 1rem;
    text-decoration: none;
    font-weight: 500;
    transition: all 0.2s;
}
.continue-btn {
    background: #8B4513;
    color: white;
}
.continue-btn:hover {
    background: #D2A679;
    [data-theme=&#34;dark&#34;] &amp; {
        color: #8B4513;
    }
}
```
5. 上述只是包含了文章页 content 部分的链接，在 `custom.js` 复制下述代码，把特殊样式以及 Waline 评论的 a 链接属性都包含。代码来自 [@空白Koobai](https://koobai.com/zhongjiantiaozhuan/)
```js

/* ------------------ 跳转风险提示 ------------------ */
document.body.addEventListener(&#39;click&#39;, function(e) {
  let target = e.target.closest(&#39;.wl-cards a,.card-link&#39;);
  if (target &amp;&amp; !target.href.includes(&#39;bulone.github.io&#39;)) {
      e.preventDefault();
      let encodedUrl = btoa(target.href);
      let url = &#39;/redirect?url=&#39; &#43; encodedUrl;
      window.open(url, &#39;_blank&#39;);
  }
});
```

## 缺点
并没有设置白名单选项，所有链接一视同仁，如果有想法可自行修改内容。

## 参考链接
1. [HUGO 外链跳转到中间页 - 空白Koobai](https://koobai.com/zhongjiantiaozhuan/)
2. [Hugo外部链接跳转提示页面](https://www.eallion.com/hugo-redirect-landing-page/)
3. [博客外链跳转到中间页 \| Solitudes](https://blog.ysicing.net/site-jump-external-link)
4. [DeepSeek \| 深度求索](www.deepseek.com)

---

> 作者: 吐司气泡  
> URL: https://bulone.github.io/posts/nyzk0nu/  

