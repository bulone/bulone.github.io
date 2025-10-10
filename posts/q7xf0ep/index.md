# 增加设备展示页


<!--more-->

==本文章使用 AI 辅助创作==[danger]

## 引言
之前就喜欢 [@张洪Heo](https://blog.zhheo.com/equipment/) 博主博客的我的装备页，模仿着主题的 Friends 模板进行修改，在 Fixit 上也增加了~~相同~~类似的装备展示页，因为设备没那么多所以也没进行分类。如果有想法直接修改 device.html 文件，不会的问 Deepseek<sub>这玩意可真好用啊，Hugo 文档较少比搜索引擎快得多。</sub>

演示地址: [设备陈列馆](/devices)

## 目录文件树
--2025 年 6 月 30 日更新 --  
将 devices.html 移到 layouts 目录下

``` { title="文件树状图"}
根目录
│
├── content
│   └── devices.md          # 内容文档
├── data
│   └── devices.yml         # 数据配置
├── layouts
│       └── devices.html    # 页面模板
└── static
    └── devices             # 存放设备图片本地文件夹
```

## 创建页面模板
代码中我增加了如果没填设备图片链接的话，就用默认图片展位。默认图片下载链接放在文章尾部，下载后放到 `static/devices` 文件夹内即可（就是放设备图的位置）。

代码中有价格的元素，若在数据文件中填写参数就显示，不填就不显示。
```html {title="layout/page/devices.html"}
{{- define "title" -}}
{{- cond (.Param "capitalizeTitles") (title .Title) .Title -}}
{{- if .Site.Params.withSiteTitle }} {{ .Site.Params.titleDelimiter }} {{ .Site.Title }}{{- end -}}
{{- end -}}

{{- define "content" -}}

{{- $params := partial "function/params.html" -}}
<article class="page single special">
    <div class="header">
        {{- /* Title */ -}}
        <h1 class="single-title animate__animated animate__pulse animate__faster">{{- cond (.Param
            "capitalizeTitles") (title .Title) .Title -}}</h1>
        {{- /* Subtitle */ -}}
        {{- with $params.subtitle -}}<p class="single-subtitle animate__animated animate__fadeIn">{{ . |
            $.RenderString }}</p>{{- end -}}
    </div>
    <div class="devices"  id="content">
        {{- $optim := slice 
            (dict "Process" "resize 800x webp q75" "descriptor" "800w")
            (dict "Process" "resize 1200x webp q75" "descriptor" "1200w")
            (dict "Process" "resize 1600x webp q75" "descriptor" "1600w")
          -}}
        {{ range $index, $device := .Site.Data.devices }}
        {{- $deviceImage := default "/device/white.webp" $device.image -}}
        <div class="device-item animate__animated animate__fadeIn">
            <figure class="device-image">
                {{- dict "Src" $deviceImage "Alt" $device.name "Title" $device.description "Caption" $device.name "Loading" "lazy" "Linked" true "OptimConfig" $optim | partial "plugin/image.html" -}}
            </figure>

            <div class="device-info">
                {{ if $device.price }}
                <div class="device-price" title="首发价 - {{ $device.price}}">{{ $device.price}}</div>
                {{ end }}
                <div class="device-title">
                    <a href="{{ $device.link | safeURL }}"
                        class="device-link {{ if not $device.link }}disabled{{ end }}">{{ $device.name}} {{ if
                        $device.link }}↗{{ end }}</a>
                </div>
                <p class="device-description">{{ $device.description }}</p>
            </div>
        </div>
        {{ end }}
    </div>

    {{- /* Content */ -}}
    <div class="content">
        {{- dict "Content" .Content "Ruby" $params.ruby "Fraction" $params.fraction "Fontawesome" $params.fontawesome |
        partial "function/content.html" | safeHTML -}}
    </div>

    {{- /* Comment */ -}}
    {{- partial "single/comment.html" . -}}

</article>
{{ end }}
```

## CSS
```SCSS {title="assest/css/_custom.scss中添加"}
/* -------------------- 设备页 ------------------- */
@import url('_https://fonts.cdnfonts.com/css/d-din_');
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
		font-family: 'D-DIN', sans-serif;
	 }
	.device-title {
	    font-size: 1rem;
	    font-weight: 600;
	    a {
	        &.disabled {
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
```yaml {title="content/devices.md"}
---
title: 设备陈列馆
subtitle: 记录曾经或正在使用过设备
layout: "devices"
---
```

| title | subtitle | layout |
| :---: | :------: | :----: |
|  标题   |   副标题    |  页面模板  |

在该模板内填写内容时会在设备列表下展示的。
## 数据格式
```yaml {title="data/devices.yml"}
- name: 设备名称
  description: 设备描述
  image: 设备图片
  link(可选): 设备链接
  price(可选): 设备价格
```
<sub>注：可选的意思是加活不加都行</sub>

以上配置完成后别忘了把图片放到 static/devices 内。图片的推荐大小为 450*300，尽量周围留有空白，保证图片完美展示。小点没关系，图片有点击放大功能。

{{< link href="/devices/white.webp" title=" 展位图下载 " content=" 设备展位图 " card=true download="white.webp" >}}

## 参考链接

1. [好物beta](https://www.eallion.com/goods/)
2. [我的装备 \| 张洪Heo](https://blog.zhheo.com/equipment/)
3. [DIN- CDNFont](https://www.cdnfonts.com/d-din.font?__cf_chl_rt_tk=nzoRSStBfMypN47isN61eMWawNeHcy8.CopGOguj1xY-1743081133-1.0.1.1-69LlfaZhF0D42wp0OyGq0SLegJwnQdLdxinvpahQRbY)
4. [Deepseek](https://www.deepseek.com)

---

> 作者: 吐司气泡  
> URL: https://blog.toastbubble.top/posts/q7xf0ep/  

