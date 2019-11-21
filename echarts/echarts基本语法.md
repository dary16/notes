#### 使用 ####
npm安装ECharts:npm install echarts --save
引入ECharts:
```html
var echarts = require('echarts');

// 基于准备好的dom，初始化echarts实例
var myChart = echarts.init(document.getElementById('main'));
// 绘制图表
myChart.setOption({
    title: {
        text: 'ECharts 入门示例'
    },
    tooltip: {},
    xAxis: {
        data: ['衬衫', '羊毛衫', '雪纺衫', '裤子', '高跟鞋', '袜子']
    },
    yAxis: {},
    series: [{
        name: '销量',
        type: 'bar',
        data: [5, 20, 36, 10, 10, 20]
    }]
});
```

#### 图表的样式 ####
绘制饼图：南丁格尔图
```html
myChart.setOption({
    series : [
        {
            name: '访问来源',
            type: 'pie',
            radius: '55%',
            data:[
                {value:235, name:'视频广告'},
                {value:274, name:'联盟广告'},
                {value:310, name:'邮件营销'},
                {value:335, name:'直接访问'},
                {value:400, name:'搜索引擎'}
            ]
        }
    ]
})
```
这里的data属性不像入门教程里那样每一项都是单个数值，而是一个包含name和value属性的对象，echarts中的数据项既可以设置成数值，也可以设成一个包含有名称、该数据图形的样式配置、标签配置的对象。
echarts中的饼图也支持通过设置roseType显示成南丁格尔图： roseType:'angle';南丁格尔图通过半径表示数据的大小

#### 阴影的配置 ####
echarts中有一些通用的样式，如阴影、透明度、颜色、边框颜色、边框宽度等，这样的样式一般都会在系列的itemStyle里设置。例如阴影的样式可以通过下面几个配置项设置：
```javascript
itemStyle: {
    // 阴影的大小
    shadowBlur: 200,
    // 阴影水平方向上的偏移
    shadowOffsetX: 0,
    // 阴影垂直方向上的偏移
    shadowOffsetY: 0,
    // 阴影颜色
    shadowColor: 'rgba(0, 0, 0, 0.5)'
}
```

itemStyle的emphasis是鼠标hover时候的高亮样式。这个示例里是正常的样式下加阴影，但是可能更多的时候是hover的时候通过阴影突出
```javascript
itemStyle: {
    emphasis: {
        shadowBlur: 200,
        shadowColor: 'rgba(0, 0, 0, 0.5)'
    }
}
```
#### 深色背景和浅色标签 ####
背景色是全局的，所以直接在option下设置backgroundColor

```javascript
setOption({
    backgroundColor: '#2c343c'
})
```
文本的样式可以设置全局的textStyle

```javascript
setOption({
     textStyle: {
        color: 'rgba(255, 255, 255, 0.3)'
    }
})
```
也可以每个系列分别设置，每个系列的文本设置在label.textStyle里
```javascript
label: {
    textStyle: {
        color: 'rgba(255, 255, 255, 0.3)'
    }
}
```

饼图的话还要将标签的视觉引导线的颜色设为浅色
```javascript
labelLine: {
    lineStyle: {
        color: 'rgba(255, 255, 255, 0.3)'
    }
}
```

跟itemStyle一样，label和labelLine的样式也有emphasis状态

#### 设置扇形的颜色 ####
扇形的颜色也是在itemStyle中设置：
```javascript
itemStyle:{
  color:'#c23523',
  shadowBlur:200,
  shadowColor:'rgba(0,0,0,0.5)'
}
```
echarts中每个扇形的颜色可以通过分别设置data下的数据项实现
```javascript
data: [{
    value:400,
    name:'搜索引擎',
    itemStyle: {
        color: '#c23531'
    }
}, ...]
```
如果只有明暗度的变化，有一种更快捷的方式是通过visualMap组件将值的大小映射到明暗度
```javascript
visualMap: {
    // 不显示 visualMap 组件，只用于明暗度的映射
    show: false,
    // 映射的最小值为 80
    min: 80,
    // 映射的最大值为 600
    max: 600,
    inRange: {
        // 明暗度的范围是 0 到 1
        colorLightness: [0, 1]
    }
}
```
