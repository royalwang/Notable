---
title: 背景大小 contain 和 cover
tags: [Html]
---

# 背景大小 contain 和 cover
CSS 的背景有下列属性:
* background-color
* background-image
* [background-position](https://www.w3schools.com/cssref/pr_background-position.asp)
* [background-size](https://www.w3schools.com/cssref/css3_pr_background-size.asp)
* [background-repeat](https://www.w3schools.com/cssref/pr_background-repeat.asp)
* [background-origin](https://www.w3schools.com/cssref/css3_pr_background-origin.asp)
* [background-clip](https://www.w3schools.com/cssref/css3_pr_background-clip.asp)
* [background-attachment](https://www.w3schools.com/cssref/pr_background-attachment.asp)

背景大小使用 [background-size](http://www.w3school.com.cn/tiy/c.asp?f=css_background-size&p=8):
* `cover`: Resize the background image to cover the entire container, even if it has to stretch the image or cut a little bit off one of the edges
  没有空白，比较适合设置为头像
* `contain`: Resize the background image to make sure the image is fully visible
* 都是等比缩放

<img src="@attachment/contain-cover.png" width=450>

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <title>Background</title>

    <style>
        .contain, .cover {
            background-position: center;
            background-repeat: no-repeat;
            background-image: url(apple.png);

            width:  200px;
            height: 200px;
            float: left;
            margin-right: 20px;
            border-radius: 6px;
            box-shadow: 0 0 2px #CCC;
        }

        .contain {
            background-size: contain;
        }

        .cover {
            background-size: cover;
        }
    </style>
</head>

<body>
    <div class="cover"></div>
    <div class="contain"></div>
</body>

</html>
```

> 为了使得效果更好，背景居中且 no-repeat。
