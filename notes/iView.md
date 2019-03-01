---
title: iView
tags: [Vue]
created: '2019-01-12T07:03:31.333Z'
modified: '2019-02-27T03:04:11.898Z'
---

# iView

## Radio
Radio 使用的时候需要注意是单独使用还是组合使用:
* 单独使用: 可以使用 v-model 绑定 Boolean 变量, 也可以设置 true-value 和 false-value 的值与其他类型的变量绑定
* 组合使用: 即放在 RadioGroup 中使用, 不能绑定 Boolean 变量, 只能绑定 String 和 Number

很常见一个需求: 二选一, 选 A 为 false, 选 B 为 true, 却不能使用 Radio 来实现, 因为有 2 个 Radio, 就需要和 RadioGroup 一起组合使用, 这时不能绑定 Boolean 变量, 这时可以考虑使用 Switch 来实现.

RadioGroup 绑定:
```html
<!-- single 为数字 -->
<RadioGroup v-model="single">
    <Radio :label="0">单选</Radio>
    <Radio :label="1">多选</Radio>
</RadioGroup>

<!-- single 为字符串 -->
<RadioGroup v-model="single">
    <Radio label="0">单选</Radio>
    <Radio label="1">多选</Radio>
</RadioGroup>
```

## Card
```html
<Card dis-hover>
    <p slot="title">Title</p>
    <p slot="extra">Extra</p>
    <div>Content of Card</div>
</Card>
```

## Card 的按钮提示
使用 Tooltip 或者 Poptip

```html
<Card dis-hover style="width:350px">
    <!-- [1] Title -->
    <p slot="title">Title</p>

    <!-- [2] Extra: tooltip -->
    <Tooltip placement="top" slot="extra">
        <Icon type="md-help-circle" />
        <div slot="content">
            <p>Display multiple lines of information</p>
            <p><i>Can customize the style</i></p>
        </div>
    </Tooltip>
    
    <!--
    <Poptip slot="extra" trigger="hover" content="Steven Paul Jobs">
        <Icon type="md-help-circle" />
    </Poptip>
    -->

    <!-- [3] Content -->
    <div>
        Content of Card
    </div>
</Card>
```
