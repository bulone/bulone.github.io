# 增加多图排版


:(fa-solid fa-copyright): 封面图片截于**Unsplash**官网
<!--more-->
==本文代码部分使用 Deepseek 修改==[danger]  

- 2025 年 11 月 10 日：增加标题功能

连续多张图片无论手机上还是电脑上的观感都挺一般，没有增加这个功能之前如果遇到多张图片的话都是用表格进行排版，不过主题原因第一行表头的背景色为深灰色，改或不改都不绝对。之前想过增添多图排版，当时的想法是将主题的图片渲染模板进行修改，不过水平不够且修改的地方也有点多于是不了了之。最近看到文档可以自定义 shortcode，就有个想法把多张连续图片放在 shortcode 中，然后在进行单独的样式就能满足要求。

## 配置 ShortCode
在根目录 `layouts` 内创建 `_shortcodes/muti-image.html` 文件，将下述代码复制粘贴。
```go-htmltemplate
<div class="muti-image-container">
  <div class="muti-image">
    {{ with .Inner }}
    {{/* 预处理：移除单独的换行符，但保留图片之间的空行 */}}
    {{ $processedContent := replace . "\n" " " }}
    {{ $processedContent = $processedContent | markdownify }}
    {{ $processedContent }}
    {{ end }}
  </div>
  {{ with .Get "title" }}
  <div class="muti-image-title">{{ . | markdownify }}</div>
  {{ end }}
</div>

```
在 `assets/css/_custom.css` 中添加下述 CSS 代码。当然也可以在 html 中也可以使用 style 块内置，具体样式还是根据自身主题进而修改。
```css
/* -------------------------------------------- */
/*                     多图测试                 */
/* ------------------------------------------- */

.muti-image {
    column-count: 2;
    column-gap: 8px;
    margin: 6px 0;
    width: 100%;
}

/* 2个链接（每个链接包含1个图片） */
.muti-image:has(> a:nth-of-type(2)) {
    column-count: 2;
    column-gap: 8px;
    margin: 6px 0;
}

.muti-image:has(> a:nth-of-type(2)) img {
    display: inherit;
}

/* 3个链接 */
.muti-image:has(> a:nth-of-type(3)) {
    column-count: 3;
}

/* 4个链接 */
.muti-image:has(> a:nth-of-type(4)) {
    column-count: 4;
}

/* 5个链接 */
.muti-image:has(> a:nth-of-type(5)) {
    column-count: 5;
}

/* 6个链接 */
.muti-image:has(> a:nth-of-type(6)) {
    column-count: 4;
}

.muti-image:has(> a:nth-of-type(6)) img {
    margin-bottom: 8px;
}

.single .muti-image-container {
    .muti-image {
        .lightgallery {
            display: inherit;
            break-inside: avoid;

            img {
                border-radius: 0rem !important;
                display: unset;
                margin: unset;
                height: unset;
                overflow: unset;
            }

        }


        figcaption {
            display: none !important;
        }
    }
    .muti-image-title{
        text-align: center;
        font-size: .875em;
        color: #969696;

    }
}
```
## 使用
> 默认
```markdown
{{</* muti-image */>}}
![]()
![]()
{{</* /muti-image */>}}
```
> 带标头
```markdown
{{</* muti-image title="支持markdown格式" */>}}
![]()
![]()
{{</* /muti-image */>}}
```
## 示例
### 带标头多图
{{< muti-image title="支持 markdown 格式" >}}  
![Travel1](./images/1.webp)  
![Travel2](./images/2.webp)    
{{< /muti-image >}}
### 两张图片
{{< muti-image >}}  
![Travel1](./images/1.webp)  
![Travel2](./images/2.webp)  
{{< /muti-image>}}

### 三张图片

{{< muti-image >}}  
![NothingPhone1](./images/index-1761479603017.webp)  
![NothingPhone2](./images/index-1761479609128.webp)  
![NothingPhone3](./images/index-1761479615431.webp)  
{{< /muti-image >}}

### 四张图片

{{< muti-image >}}  
![NothingPhone3](./images/index-1761479615431.webp)  
![NothingPhone2](./images/index-1761479609128.webp)  
![NothingPhone1](./images/index-1761479603017.webp)  
![NothingPhone4](./images/index-1761619783313.webp)  
{{< /muti-image >}}
### 多张图片

{{< muti-image >}}  
![NothingPhone2](./images/index-1761479609128.webp)  
![Travel2](./images/2.webp)  
![大熊猫](images/panada.webp)  
![重庆](images/chongqing.webp)  
![NothingPhone4](./images/index-1761619783313.webp)  
![Chicken](./images/chicken.webp)  
![春节](./images/spring_festival.webp)  
{{< /muti-image >}}  
~注: 本文章图片以进行压缩处理~

## 参考
 1. 样式参考：[Hugo 多图排版这样来 :: 木木木木木](https://immmmm.com/about-images-gird/)
 2. 图片资源：[Unplash](https://unsplash.com/)
 3. 长图片：[NothingPhone壁纸](https://nothingcn.com/pages/phone-2)

---

> 作者: 吐司气泡  
> URL: https://blog.toastbubble.top/posts/78yen8g/  

