---
title: Table Echarts
tags: [Vue]
created: '2019-03-12T14:06:51.654Z'
modified: '2019-03-12T14:14:41.312Z'
---

# Table Echarts

在 Table 中显示图表:
* 给要显示 echarts 图表的每个单元格设置唯一 id
* 当数据设置完后，在下一个事件中创建图表

```js
trendColumns: [
    { title: '姓名', key: 'nickname', width: 100 },
    { title: '近五次排名变化', render: (h, params) => {
        const ranks = params.row.ranks; // 排名数组
        const id    = 'rank-chart-' + Utils.nextId(); // 唯一 id

        // 下一个事件中显示图表
        this.$nextTick(() => {
            const chartDom = document.getElementById(id);
            if (!chartDom) { return; }

            const chart = echarts.init(chartDom);
            chart.setOption(this.trendChartOption); // 设置默认的设置
            chart.setOption({
              series: [ { data: ranks } ] // 设置数据
            });
        });

        return (
            <div id={ id } style="width: 300px; height: 50px;" class="trend-chart"></div>
        );
    } },
]
```
