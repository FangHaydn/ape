# ECharts

[官方文档](https://echarts.apache.org/zh/index.html)


```javascript
var echart = echarts.init(DOM: dom, String: theme)
echart.showLoading
echart.hideLoading
echart.setOption({
    xAxis,
    yAxis,
    tooltip: {
        trigger: 'click',
        formatter: function(){return ''}
    },
    toolbox: {
        feature: []
    }
    series: [
        {
            type: 'bar', // bar line scatter radar pie
            data
        }
    ]
})

```