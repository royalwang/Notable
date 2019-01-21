---
title: Grid 布局
tags: [Html]
---

# Grid 布局
Grid 是二维的布局, 同时有行和列时使用, Flex 是一维布局, 只有行或者列时使用, Grid 学习可参考
* [5 分钟学会 CSS Grid 布局](https://www.css88.com/archives/8506)
* [如何使用 CSS Grid 快速而又灵活的布局](https://www.css88.com/archives/8512)
* [CSS Grid 布局完全指南](https://www.css88.com/archives/8510)
* [Grid 布局完整指南](https://segmentfault.com/a/1190000012889793)

Grid Container 的全部属性:
* display: `grid | inline-grid` (block 和行内)
* grid-template-columns
* grid-template-rows
* grid-template-areas: 给网格命名, 语言上便于阅读, 和 item 的 grid-area 一起使用
* grid-template
* grid-column-gap
* grid-row-gap
* grid-gap
* justify-items: `start | end | center | stretch (default)`
* align-items: `start | end | center | stretch (default)`
* justify-content: `start | end | center | stretch | space-around | space-between | space-evenly`
* align-content: `start | end | center | stretch | space-around | space-between | space-evenly`
* grid-auto-columns
* grid-auto-rows
* grid-auto-flow: `row (default) | column | row dense | column dense` (dense 可能导致 grid item 乱序)
* grid

Grid Items 的全部属性:
* grid-column-start
* grid-column-end
* grid-row-start
* grid-row-end
* grid-column: `grid-column-start + grid-column-end` 的简写, 属性值之间用 `/` 分隔
* grid-row: 可以使用 span, 如 1 / span 3
* grid-area
* justify-self: `start | end | center | stretch`
* align-self: `start | end | center | stretch`

相关概念:
* grid container
* grid item
* grid line
* grid track

```
创建一个 12 列的网格，每个列都是一个单位宽度 (总宽度的 1/12), fr 可以理解为 free space
grid-template-columns: repeat(12, 1fr);
```

## 经典的上中下布局:
<img src="@attachment/grid-classic.png" width=658>

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <title>Grid</title>
    <style media="screen">
        .header  { background: #2d8cf0; }
        .sidebar { background: #515a6e; }
        .content { background: #808695; }
        .footer  { background: #5cadff; }

        .wrapper {
            display: grid;
            grid-template-columns: 200px auto;
            grid-template-rows: 50px 200px 50px;
        }

        .header, .footer {
            grid-column-start: 1;
            grid-column-end: 3;
        }
    </style>
</head>

<body>
    <div class="wrapper">
        <div class="header"></div>
        <div class="sidebar"></div>
        <div class="content"></div>
        <div class="footer"></div>
    </div>
</body>

</html>
```

使用 grid-template-areas:
```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <title>Grid</title>
    <style media="screen">
        .header  { background: #2d8cf0; }
        .sidebar { background: #515a6e; }
        .content { background: #808695; }
        .footer  { background: #5cadff; }

        .wrapper {
            display: grid;
            grid-template-columns: repeat(12, 1fr);
            grid-template-rows: 50px auto 50px;
            grid-template-areas:
                "h h h h h h h h h h h h"
                "s s s c c c c c c c c c"
                "f f f f f f f f f f f f";
            height: 400px;
        }

        @media screen and (max-width: 540px) {
            .wrapper {
                grid-template-areas:
                    "s s s h h h h h h h h h"
                    "c c c c c c c c c c c c"
                    "f f f f f f f f f f f f";
            }
        }

        .header {
            grid-area: h;
        }

        .sidebar {
            grid-area: s;
        }

        .content {
            grid-area: c;
        }

        .footer {
            grid-area: f;
        }
    </style>
</head>

<body>
    <div class="wrapper">
        <div class="header">Header</div>
        <div class="sidebar">Sidebar</div>
        <div class="content">Content</div>
        <div class="footer">Footer</div>
    </div>
</body>

</html>
```

## 更自由的布局:

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <title>Grid</title>
    <style media="screen">
        .red    { background: red; }
        .green  { background: green; }
        .blue   { background: blue; }
        .orange { background: orange; }
        .yellow { background: yellow; }
        .pink   { background: pink; }

        .wrapper {
            display: grid;
            grid-template-columns: 200px auto 100px;
            grid-template-rows: 50px 300px 50px;
        }

        .item1 {
            grid-column-start: 2;
            grid-column-end: 4;
        }

        .item2 {
            grid-row-start: 1;
            grid-row-end: 3;
        }

        .item5 {
            grid-column-start: 1;
            grid-column-end: 4;
        }
    </style>
</head>

<body>
    <div class="wrapper">
        <div class="item item1 red">1</div>
        <div class="item item2 green">2</div>
        <div class="item item3 blue">3</div>
        <div class="item item4 orange">4</div>
        <div class="item item5 yellow">5</div>
        <div class="item item6 pink">6</div>
    </div>
</body>

</html>
```
