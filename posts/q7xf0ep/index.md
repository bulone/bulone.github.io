# 增加设备展示页


&lt;!--more--&gt;

==本文章使用 AI 辅助创作==[danger]
## 引言
之间就馋 [@张洪Heo](https://blog.zhheo.com/equipment/) 博主博客的我的装备页，模仿着主题的 Friends 模板进行修改，在 Fixit 上也增加了~~相同~~类似的装备展示页，因为设备没那么多所以也没进行分类。如果有想法直接修改 device.html 文件，不会的问 Deepseek&lt;sub&gt;这玩意可真好用啊，Hugo 文档较少比搜索引擎快得多。&lt;/sub&gt;

演示地址: [设备陈列馆](/devices)

## 目录文件树

``` { title=&#34;文件树状图&#34;}
根目录
│
├── content
│   └── devices.md          # 内容文档
├── data
│   └── devices.yml         # 数据配置
├── layout
│   └── page
│       └── devices.html    # 页面模板
└── static
    └── devices             # 存放设备图片本地文件夹
```

## 创建页面模板
代码中我增加了如果没填设备图片链接的话，就用默认图片展位。默认图片下载链接放在文章尾部，下载后放到 `static/devices` 文件夹内即可（就是放设备图的位置）。

代码中有价格的元素，若在数据文件中填写参数就显示，不填就不显示。
```html {title=&#34;layout/page/devices.html&#34;}
{{- define &#34;title&#34; -}}
{{- cond (.Param &#34;capitalizeTitles&#34;) (title .Title) .Title -}}
{{- if .Site.Params.withSiteTitle }} {{ .Site.Params.titleDelimiter }} {{ .Site.Title }}{{- end -}}
{{- end -}}

{{- define &#34;content&#34; -}}

{{- $params := partial &#34;function/params.html&#34; -}}
&lt;article class=&#34;page single special&#34;&gt;
    &lt;div class=&#34;header&#34;&gt;
        {{- /* Title */ -}}
        &lt;h1 class=&#34;single-title animate__animated animate__pulse animate__faster&#34;&gt;{{- cond (.Param
            &#34;capitalizeTitles&#34;) (title .Title) .Title -}}&lt;/h1&gt;
        {{- /* Subtitle */ -}}
        {{- with $params.subtitle -}}&lt;p class=&#34;single-subtitle animate__animated animate__fadeIn&#34;&gt;{{ . |
            $.RenderString }}&lt;/p&gt;{{- end -}}
    &lt;/div&gt;
    &lt;div class=&#34;devices&#34;  id=&#34;content&#34;&gt;
        {{- $optim := slice 
            (dict &#34;Process&#34; &#34;resize 800x webp q75&#34; &#34;descriptor&#34; &#34;800w&#34;)
            (dict &#34;Process&#34; &#34;resize 1200x webp q75&#34; &#34;descriptor&#34; &#34;1200w&#34;)
            (dict &#34;Process&#34; &#34;resize 1600x webp q75&#34; &#34;descriptor&#34; &#34;1600w&#34;)
          -}}
        {{ range $index, $device := .Site.Data.devices }}
        {{- $deviceImage := default &#34;/device/white.webp&#34; $device.image -}}
        &lt;div class=&#34;device-item animate__animated animate__fadeIn&#34;&gt;
            &lt;figure class=&#34;device-image&#34;&gt;
                {{- dict &#34;Src&#34; $deviceImage &#34;Alt&#34; $device.name &#34;Title&#34; $device.description &#34;Caption&#34; $device.name &#34;Loading&#34; &#34;lazy&#34; &#34;Linked&#34; true &#34;OptimConfig&#34; $optim | partial &#34;plugin/image.html&#34; -}}
            &lt;/figure&gt;

            &lt;div class=&#34;device-info&#34;&gt;
                {{ if $device.price }}
                &lt;div class=&#34;device-price&#34; title=&#34;首发价 - {{ $device.price}}&#34;&gt;{{ $device.price}}&lt;/div&gt;
                {{ end }}
                &lt;div class=&#34;device-title&#34;&gt;
                    &lt;a href=&#34;{{ $device.link | safeURL }}&#34;
                        class=&#34;device-link {{ if not $device.link }}disabled{{ end }}&#34;&gt;{{ $device.name}} {{ if
                        $device.link }}↗{{ end }}&lt;/a&gt;
                &lt;/div&gt;
                &lt;p class=&#34;device-description&#34;&gt;{{ $device.description }}&lt;/p&gt;
            &lt;/div&gt;
        &lt;/div&gt;
        {{ end }}
    &lt;/div&gt;

    {{- /* Content */ -}}
    &lt;div class=&#34;content&#34;&gt;
        {{- dict &#34;Content&#34; .Content &#34;Ruby&#34; $params.ruby &#34;Fraction&#34; $params.fraction &#34;Fontawesome&#34; $params.fontawesome |
        partial &#34;function/content.html&#34; | safeHTML -}}
    &lt;/div&gt;

    {{- /* Comment */ -}}
    {{- partial &#34;single/comment.html&#34; . -}}

&lt;/article&gt;
{{ end }}
```

## CSS
```SCSS {title=&#34;assest/css/_custom.scss中添加&#34;}
/* -------------------- 设备页 ------------------- */
@import url(&#39;_https://fonts.cdnfonts.com/css/d-din_&#39;);
.devices {
    display: flex;
    flex-wrap: wrap;
    justify-content: left;
    gap: 12px;
    margin-top: 1rem;
    box-sizing: border-box;
}

.device-item {
    border-radius: 6px;
    overflow: hidden;
    box-shadow: 0 3px 6px rgba(0, 0, 0, 0.1);
    flex: 0 0 calc((100% - 30px) / 3);
    transition: transform 0.3s ease;
    border: solid 1px #f0f0f0;
    border-radius: 1rem;
}

.device-image {
    height: 150px;
    /* 减小高度 */
    overflow: hidden;
    margin: 0;
}

.device-image img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform 0.3s ease;
}

.device-item:hover .device-image img {
    transform: scale(1.05);
}

.device-info {
    padding: 12px;
    text-align: left;
	.device-price {
		font-size: .875rem;
		line-height: 1.4;
		font-family: &#39;D-DIN&#39;, sans-serif;
	 }
	.device-title {
	    font-size: 1rem;
	    font-weight: 600;
	    a {
	        &amp;.disabled {
	            pointer-events: none;
	        }
	    }
	}
    p {
        font-size: 13px;
        line-height: 1.4;
    }
}

@media only screen and (max-width: 760px) {
	.devices {
        gap: 1rem;
	}
    .device-item {
		flex: 0 0 calc((100% - 1rem-4px) / 2);
	    /* 非常小的屏幕上两列布局 */
    }
}

@media only screen and (max-width: 480px) {
	.device-item {
        flex: 0 0 100%;
        /* 非常小的屏幕上两列布局 */
        }
    .device-image {
        height: 200px;
    }
}

```

## MD
```yaml {title=&#34;content/devices.md&#34;}
---
title: 设备陈列馆
subtitle: 记录曾经或正在使用过设备
layout: &#34;devices&#34;
---
```

| title | subtitle | layout |
| :---: | :------: | :----: |
|  标题   |   副标题    |  页面模板  |

在该模板内填写内容时会在设备列表下展示的。
## 数据格式
```yaml {title=&#34;data/devices.yml&#34;}
- name: 设备名称
  description: 设备描述
  image: 设备图片
  link(可选): 设备链接
  price(可选): 设备价格
```
&lt;sub&gt;注：可选的意思是加活不加都行&lt;/sub&gt;

以上配置完成后别忘了把图片放到 static/devices 内。图片的推荐大小为 450*300，尽量周围留有空白，保证图片完美展示。小点没关系，图片有点击放大功能。

{{&lt; link href=&#34;/devices/white.webp&#34; title=&#34; 展位图下载 &#34; content=&#34; 设备展位图 &#34; card=true download=&#34;white.webp&#34; &gt;}}

## 参考链接

1. [好物beta](https://www.eallion.com/goods/)
2. [我的装备 \| 张洪Heo](https://blog.zhheo.com/equipment/)
3. [DIN- CDNFont](https://www.cdnfonts.com/d-din.font?__cf_chl_rt_tk=nzoRSStBfMypN47isN61eMWawNeHcy8.CopGOguj1xY-1743081133-1.0.1.1-69LlfaZhF0D42wp0OyGq0SLegJwnQdLdxinvpahQRbY)
4. [Deepseek](https://www.deepseek.com)

---

> 作者: 吐司气泡  
> URL: https://blog.toastbubble.top/posts/q7xf0ep/  

