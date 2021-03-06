# echarts tooltip carousel
echarts中tooltip自动轮播工具

## api
function loopShowTooltip (chart, chartOption, options);

参数| 说明
---|---
chart | ECharts实例
chartOption | ECharts配置信息
options | {<br>interval:轮播时间间隔，单位毫秒，默认为2000 <br> loopSeries:  boolean类型，默认为false。true表示循环所有series的tooltip；false则显示指定seriesIndex的tooltip。 <br> seriesIndex: 默认为0，指定某个系列（option中的series索引）循环显示tooltip，当loopSeries为true时，从seriesIndex系列开始执行。 <br> updateData:  自定义更新数据的函数，默认为null；用于类似于分页的效果，比如总数据有20条，chart一次只显示5条，全部数据可以分4次显示。 <br> }
返回值：| {clearLoop: clearLoop} 取消轮播

## 例子
[example](https://github.com/chengwubin/echarts-tooltip-cyclic-display/blob/master/examples.html)<br>
```JavaScript
  // 基于准备好的dom，初始化echarts图表
  let chart = echarts.init(document.getElementById('id'));
  let chartOption = {
    // ...
  };

  // 为echarts对象加载数据
  chart.setOption(chartOption);
    
  // 调用本工具接口
  tools.loopShowTooltip(chart, chartOption, {
    loopSeries: true
  });
```
### 更新数据
```JavaScript
  let data = [{
    name: '2000',
     value: 55
  }, {
    name: '2001',
    value: 67
  }, {
    name: '2002',
    value: 60
  }, {
    name: '2003',
    value: 73
    }, {
    name: '2004',
    value: 93
  }, {
    name: '2005',
    value: 100
  }, {
    name: '2006',
    value: 220
  }, {
    name: '2007',
    value: 220
  }, {
    name: '2008',
    value: 220
  }, {
    name: '2009',
    value: 220
  }, {
    name: '2010',
    value: 220
  }];
  
  let currentPage = 0; // 当前页
  let counts = 2; // 每页条数
  let total = data.length; // 总数
  let totalPage = Math.ceil(total / counts); // 总页数
  let chartOption = {
          tooltip: {
            trigger: 'axis'
          },
          xAxis: {
            type: 'category',
            data: []
          },
          yAxis: {
            type: 'value'
          },
          series: [{
            data: [],
            type: 'bar'
          }]
    };
  
  function updateData() {
    let xAxisData = [];
    let seriesData = [];
  
    let currentData = data.slice(currentPage * counts, (currentPage + 1) * counts);
    currentData.forEach(function (item) {
      xAxisData.push(item.name);
      seriesData.push(item.value);
    });
  
    currentPage = ++currentPage % totalPage;
  
    bar.xAxis.data = xAxisData;
    bar.series[0].data = seriesData;
  }
 
  updateData();

  // 基于准备好的dom，初始化echarts图表
  let chart = echarts.init(document.getElementById('id'));
 
  // 为echarts对象加载数据
  chart.setOption(chartOption);
    
  // 调用本工具接口
  tools.loopShowTooltip(chart, chartOption, {
    loopSeries: true,
    updateData: updateData
  });
```

### 效果展示
![image](https://github.com/chengwubin/echarts-tooltip-cyclic-display/blob/master/scatter.gif)
![image](https://github.com/chengwubin/echarts-tooltip-cyclic-display/blob/master/line.gif)
![image](https://github.com/chengwubin/echarts-tooltip-cyclic-display/blob/master/bar.gif)
![image](https://github.com/chengwubin/echarts-tooltip-cyclic-display/blob/master/update.gif)


## License
[MIT](https://github.com/chengwubin/echarts-tooltip-cyclic-display/blob/master/LICENSE)
