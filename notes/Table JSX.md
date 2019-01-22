---
title: Table JSX
tags: [Vue]
---

# Table JSX
使用 JSX 渲染复杂的 Table cell

```html
<Table :columns="columns" :data="dicts" style="margin: 10px 0;"></Table>
```

```js
dicts  : [], // 字典表的数据
columns: [   // 字典表的列名
    { title: '编码', key: 'code', width: 200 },
    { title: '字段值', key: 'value', render: (h, params) => {
        const dict  = params.row; // 字典对象
        let image = null; // null 时不会生成 DOM

        if (dict.description.toUpperCase().includes('URL')) {
            image = <img src={ dict.value } style="display: block; max-width: 150px;"/>;
        }

        return (
            <div>{ image }<span>{ dict.value }</span></div>
        );
    } },
    { title: '字段类型', key: 'type', width: 160 },
    { title: '备注', key: 'description' },
    { title: '操作', key: 'action', width: 160, align: 'center', render: (h, params) => {
        // 编辑和禁用按钮
        return (
            <div class="cell-button-container">
                <i-button ghost type="text" shape="circle" icon="md-close-circle" onClick={ () => { this.deleteDict(params.index); }}></i-button>
            </div>
        );
    } }
]
```
提示:
* title 是表头
* key 的值是要渲染的对象的属性
* `params.index` 为要渲染的对象在数组 dicts 中的下标
* `params.row` 为要渲染的对象，即数组 dicts 中的元素
* [JSX 可以使用条件判断](https://segmentfault.com/a/1190000007797584)

以下组件，在非 template，或者在 render 模式下，需要加前缀 `i-`，其他的不需要加：
* Button: `i-button`
* Col: `i-col`
* Table: `i-table`
* Input: `i-input`
* Form: `i-form`
* Menu: `i-menu`
* Select: `i-select`
* Option: `i-option`
* Progress: `i-progress`
* Time: `i-time`

其他的组件在非 template，或者在 render 模式下，不需要加前缀 `i-`，例如 Dropdown，在 render 中使用 `dropdown` 而不是 `i-dropdown`

以下组件，在所有模式下，必须加前缀 i-，除非使用 iview-loader：
* Switch: i-switch
* Circle: i-circle