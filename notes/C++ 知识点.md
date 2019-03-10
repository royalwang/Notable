---
title: C++ 知识点
tags: [Qt]
created: '2019-03-03T02:22:46.570Z'
modified: '2019-03-03T02:35:29.902Z'
---

# C++ 知识点
[C++ 11](http://blog.jobbole.com/44015/) 特性

## extern
`extern "C" void fun(int a,int b)` 告诉编译器在编译 fun 这个函数时候按着 C 的规矩去翻译，而不是 C++ 的。

## 全局变量
1. 在 x.c 中定义全局变量 PI: `extern double PI = 3.1415;`
2. 在 x.h 中声明全局变量 PI: `extern double PI;`
3. 在文件中包含 x.h: `#include "x.h";`
4. 使用全局变量 PI: `double area = PI * r * r;`

> 如果在 x.h 中定义全局变量 PI 的话，x.h 被多次包含时会抛出重复定义的编译错误。


## 强类型枚举

传统的 C++ 枚举类型存在一些缺陷：它们会将枚举常量暴露在外层作用域中（这可能导致名字冲突，如果同一个作用域中存在两个不同的枚举类型，但是具有相同的枚举常量就会冲突），而且它们会被隐式转换为整形，无法拥有特定的用户定义类型。

在 C++11 中通过引入了一个称为强类型枚举的新类型，修正了这种情况。强类型枚举由关键字 `enum class` 标识。它不会将枚举常量暴露到外层作用域中，也不会隐式转换为整形，并且拥有用户指定的特定类型 (传统枚举也增加了这个性质)
```cpp
enum class Option {
    One, Two, Three
}

Option o = Option::One;
```
