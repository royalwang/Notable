---
title: DOM style
tags: [Html]
created: '2019-01-25T01:56:39.751Z'
modified: '2019-03-02T09:42:13.351Z'
---

# DOM style

[HTML DOM Document 对象](http://www.w3school.com.cn/jsref/dom_obj_document.asp)

## Style 对象
Style 对象表示一个个别的样式声明。

## 访问 Style 对象
Style 对象可以从文档的头部区域访问，或者从指定的 HTML 元素访问。

从文档的头部区域访问 style 对象：

```js
var x = document.getElementsByTagName("STYLE");
```

访问一个指定元素的 style 对象：

```js
var x = document.getElementById("myH1").style;
```

## 创建 Style 对象
您可以使用 document.createElement() 方法来创建 `<style>` 元素：

```js
var x = document.createElement("STYLE");
```

您也可以设置一个已有元素的 style 属性：

```
document.getElementById("myH1").style.color = "red";
```

## Style 对象属性
[请点击此处访问](http://techbrood.com/jsref?p=dom-obj-style)
