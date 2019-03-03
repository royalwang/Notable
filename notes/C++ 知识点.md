---
title: C++ 知识点
tags: [Qt]
created: '2019-03-03T02:22:46.570Z'
modified: '2019-03-03T02:27:17.388Z'
---

# C++ 知识点

## extern
`extern "C" void fun(int a,int b)` 告诉编译器在编译 fun 这个函数时候按着 C 的规矩去翻译，而不是 C++ 的。

## 全局变量
1. 在 x.c 中定义全局变量 PI: `extern double PI = 3.1415;`
2. 在 x.h 中声明全局变量 PI: `extern double PI;`
3. 在文件中包含 x.h: `#include "x.h";`
4. 使用全局变量 PI: `double area = PI * r * r;`

> 如果在 x.h 中定义全局变量 PI 的话，x.h 被多次包含时会抛出重复定义的编译错误。
