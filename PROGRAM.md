
# 量化策略脚本指南

![framework](https://user-images.githubusercontent.com/2844717/125124322-7a337200-e12a-11eb-8028-d2dc58dc3ec9.png)

## 可调用全局对象

### indicator - 指标分析对象（MA、MACD、BOLL） 

#### MA-移动平均线

```js
  /**
   * 
   * @param {number[]} close - K线收盘价数组
   * @param {number} n - 均线周期值
   * @return {number[]} 均线值数组
   */
  indicator.ma(close, n)
```

#### MACD-异同移动平均线

```js
  /**
   * 
   * @param {number[]} close - K线收盘价数组
   * @param {number} fast - 短周期值 (默认值 12)
   * @param {number} slow - 长周期值 (默认值 26)
   * @param {number} mid - 移动平均周期值 (默认值 9)
   * @return { dif: number[], dea: number[], bar: number[] } MACD
   */
  indicator.macd(close, fast = 12, slow = 26, mid = 9)
```

#### BOLL-布林带

```js
  /**
   * 
   * @param {number[]} close - K线收盘价数组
   * @param {number} size - 均线周期值 (默认值 20)
   * @param {number} times - 标准差倍数 (默认值 2)
   * @return { upper: number[], mid: number[], lower: number[] } BOLL
   */
  indicator.boll(close, size = 20, times = 2)
```

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
