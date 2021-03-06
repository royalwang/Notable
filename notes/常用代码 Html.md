---
title: 常用代码 Html
tags: [Html]
created: '2019-01-25T22:14:46.212Z'
modified: '2019-02-27T05:28:23.679Z'
pinned: true
---

# 常用代码 Html

## 仿射变换
[2D 仿射变换属性](https://segmentfault.com/a/1190000004460780)：变换 (transform)、过渡 (transition):
* transform 是状态，有 4 个变换: translate, rotate, scale, skew (倾斜)
* transition 是过程，有 4 个属性: property, duration, easing-curve, delay

```html
.trans {
    color: white;
    background: blueviolet;
    transform : translate(50px, 0) rotate(45deg) scale(1, 2);
    transition: background 1.4s ease-in-out, color 1.4s ease-out, transform .4s;
}
```
> transition 的每个属性可以用逗号分开单独写，也可以使用 `transition: all .4s` 设置所有的过渡属性。
> 变换的效果不会影响其他 DOM 元素的位置、大小等。

变换中心 transform-origin 默认为 DOM 元素的中心，可以修改，例如下面的效果为绕元素的左上角旋转 90°:
```css
transform-origin: left top;
transform : rotate(90deg);
```

## [CSS: 换行和省略号](https://blog.csdn.net/liuyan19891230/article/details/50969393)
```css
/* 强制不换行 */
p { white-space: nowrap; }

/* 自动换行 */
p { word-wrap: break-word; }

/* 强制英文单词断行: 设置强制将英文单词断行，需要将行内元素设置为块级元素 */
p { word-break: break-all; }

/* 超出显示省略号 */
p { 
    text-overflow: ellipsis;
    overflow: hidden;
}
```
