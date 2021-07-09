
# 量化策略脚本开发指南

![framework](https://user-images.githubusercontent.com/2844717/125124322-7a337200-e12a-11eb-8028-d2dc58dc3ec9.png)


## 可调用全局对象

### indicator - 指标分析对象（MA、MACD、BOLL） 
### http - 远程接口请求对象 (支持GET和POST请求)
### webhook - webhook通知对象 (支持通知到企业微信)
### joinquant - 聚宽本地数据对象 (拉取各周期K线)
### toast - UI提示信息对象
### log - DevTools调试信息对象

## 传参数据对象

### ohlcv - 当前标的TICK数据
### klines - 当前标的周期K线数据
### twist - 壹缠数据对象 (笔段中枢走势数据)
### echartsCustomSeries - 图表绘图配置数组 (参见 https://echarts.apache.org/zh/option.html#series)
