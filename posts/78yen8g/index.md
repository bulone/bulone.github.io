# 增加多图排版功能


<!--more-->
==本文代码部分使用 Deepseek 生成==[danger]  

连续多张图片无论手机上还是电脑上的观感都挺一般，没有增加这个功能之前如果遇到多张图片的话都是用表格进行排版，不过主题原因第一行表头的背景色为深灰色，改或不改都不绝对。之前想过增添多图排版，当时的想法是将主题的图片渲染模板进行修改，不过水平不够且修改的地方也有点多于是不了了之。最近看到文档可以自定义 shortcode，就有个想法把多张连续图片放在 shortcode 中，然后在进行单独的样式就能满足要求。

## 配置 ShortCode
在根目录 `layouts` 内创建 `_shortcodes/muti-image.html` 文件，将下述代码复制粘贴。
```html
{{/* 解析内部内容中的图片 */}}
{{ $content := .Inner }}
{{ $class := "muti-image" }}

<div class="{{ $class }}">
  {{ $content | markdownify }}
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

.muti-image {
    a {
        // width: 100%;
        display: inherit;
        break-inside: avoid;
        // margin-bottom: 8px;
    }

    img {
        // width: 100%;
        // display: block;
        border-radius: 0.25rem !important;
        box-shadow: 3px 3px 7px 0px #dcdcdc;

        @include dark-theme {
            background: #4e4e4e;
            box-shadow: 3px 3px 7px 0px #484848;
        }
    }
}
```
## 使用
在文档内以下格式使用，注意图片之间 **可以换行但不空行**，每行之后也不能增加两个空格，否则会被识别成单独一列。
```markdown
{{</* muti-image */>}}
![]()
![]()
{{</* /muti-image */>}}
```

## 示例
### 两张图片
{{< muti-image >}}      
![Travel1](./images/1.webp)
![Travel2](images/2.webp)
{{< /muti-image >}}

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
![NothingPhone3](./images/index-1761479615431.webp)
![NothingPhone2](./images/index-1761479609128.webp)
{{< /muti-image >}}
### 多张图片

{{< muti-image >}}  
![NothingPhone2](./images/index-1761479609128.webp)
![Travel2](images/2.webp)
![大熊猫](./images/Inserted%20image%2020251026195519.webp)
![NothingPhone3](./images/index-1761479615431.webp)
![Travel2](images/2.webp)
![Travel1](./images/1.webp)
![大熊猫](./images/Inserted%20image%2020251026195519.webp)
![NothingPhone2](./images/index-1761479609128.webp)
{{< /muti-image >}}

## 参考
 1. [Hugo 多图排版这样来 :: 木木木木木](https://immmmm.com/about-images-gird/)

---

> 作者: 吐司气泡  
> URL: https://blog.toastbubble.top/posts/78yen8g/  

