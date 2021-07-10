
# 量化策略脚本编程指南

![framework](https://user-images.githubusercontent.com/2844717/125124322-7a337200-e12a-11eb-8028-d2dc58dc3ec9.png)

## 策略脚本生命周期接口

以下策略脚本生命周期接口 用户可以根据功能需要 自行灵活实现 不需要的接口 可以不声明和实现

### 策略脚本回调接口 - TICK更新

毫秒级别tick推送时更新 (注意此接口为高频回调接口 不要在此接口处理耗时或打印操作)

```js
  /**
   * 
   * @param {string} symbol - 当前图表浏览标的代码
   * @param {object} ohlcv - TICK数据对象 (格式 { open: xxxx.xx, high: xxxx.xx, low: xxxx.xx, close: xxxx.xx, volume: xxxxxxx })
   * @return {void} 
   */
  async function tick(symbol, ohlcv) {
    // ... 
  }
```

### 策略脚本回调接口 - K线更新

当前标的周期K线更新时触发 (单个K线周期内3~10s更新一次)

```js
  /**
   * 
   * @param {string} symbol - 当前图表浏览标的代码
   * @param {string} level - K线周期 (sec5、sec15、sec30、min1、min5、min15、min30、min60、min120、min240、day、week、month)
   * @param {Kline[]} klines - K线数据对象 (格式 { day: 'YYYY-MM-DD HH:mm:ss' | 'YYYY-MM-DD', open: xxxx.xx, high: xxxx.xx, low: xxxx.xx, close: xxxx.xx, volume: xxxxxxx })
   * @param {object} twist - 壹缠数据对象 (笔段中枢走势数据 详见后表)
   * @param {array} echartsCustomSeries - 图表绘图配置数组 (详见 https://echarts.apache.org/zh/option.html#series)
   * @return {void} 
   */
  async function kline(symbol, level, klines, twist, echartsCustomSeries) {
    // ... 
  }
```

#### Twist壹缠数据对象

```js
  {
      typing: [Typing],   // 分型数据

      stroke: [Stroke],   // 笔数据
      segment: [Segment], // 段数据
      trend: [Trend],     // 走势数据
  
      strokebox: [StrokeBox],   // 笔中枢数据
      segmentbox: [SegmentBox], // 段中枢数据
  }
```

##### Typing - 分型数据结构

```js
  class Typing {

    // 类型 'top' | 'bottom'
    type 

    // 分型顶点
    point = [ 'day', 'value' ]

    // 分型顶点K线
    kline 

    // 分型顶点K线索引
    kindex

    // 提示信息 (背驰|小转大|MACD|买卖点| ...)
    tip

    // 索引值
    index

    // 时间戳
    timestamp
  }
```

##### Stroke - 笔数据结构

```js
  class Stroke {

    // 方向 'up' | 'down'
    type 

    // 起始分型
    styping

    // 结束分型
    etyping

    // 提示信息 (背驰|小转大|买卖点| ...)
    tip

    // MACD值
    macd

    // 索引值
    index

    // 时间戳
    timestamp
  }
```

##### Segment - 段数据结构

```js
export class Segment {

  // 方向 'up' | 'down'
  type 

  // 起始笔
  sstroke

  // 结束笔
  estroke

  // 笔中枢集合
  box = []

  // 提示信息 (背驰|小转大|MACD|买卖点| ...)
  tip

  // MACD值
  macd
  
  // 索引值
  index

  // 时间戳
  timestamp
}
```

##### Trend - 走势数据结构

```js

export class Trend {

  // 方向 'up' | 'down'
  type 

  // 起始段
  sstroke

  // 结束段
  estroke

  // 段中枢集合
  box = []
  
  // 提示信息 (背驰|小转大|MACD|买卖点| ...)
  tip

  // MACD值
  macd
  
  // 索引值
  index

  // 时间戳
  timestamp
}

```

##### StrokeBox/SegmentBox - 中枢数据结构

```js
  class Box {

    // 方向 'up' | 'down'
    type 

    // 中枢区间
    range = {
      // 中枢上沿
      top: 0,
      // 中枢下沿
      bottom: 0,

      // 中枢最高点 (在中枢外)
      high: 0,
      // 中枢最低点 (在中枢外)
      low: 0,
    }

    // 起始段
    ssegment

    // 结束段
    esegment 

    // 中枢段集合
    segments = [] 

    // 是否为扩张中枢
    isExpand

    // 是否为更大级别中枢（中枢段数量大于9）
    isLevelUp

    // 提示信息 (扩张|九段| ...)
    tip

    // 索引值
    index

    // 时间戳
    timestamp
  }
```

#### ECharts绘图配置数组

用户可以根据绘图需要 扩展此配置数组 详见 https://echarts.apache.org/zh/option.html#series

示例 (图表增加10日均线)

```js
   echartsCustomSeries.push({
      name: "MA10",
      type: "line",
      symbol: "none",
      data: [均线数据],
      lineStyle: {
          color: '#fc97af',
          width: 1,
      },
      emphasis: {
          lineStyle: {
            width: 1,
          }
      },
   })
```

更多配置示例 可参见 壹缠策略脚本编辑器 -> 示例脚本

### 策略脚本生命周期函数 - 脚本加载(脚本加载到图表后触发)

```js
  async function create() {
    // ...
  }
```

### 策略脚本生命周期函数 - 脚本更新(当前图表标的或周期更换时触发)

```js
  /**
   * 
   * @param {string} symbol - 当前图表浏览标的代码
   * @param {string} level - K线周期 (sec5、sec15、sec30、min1、min5、min15、min30、min60、min120、min240、day、week、month)
   * @return {void} 
   */
  async function update(symbol, level) {
    // ...
  }
```

### 策略脚本生命周期函数 - 脚本卸载(脚本停用从图表卸载后触发)

```js
  async function destroy() {
    // ...
  }
```

## 可调用全局对象

### indicator - 指标分析对象（MA、MACD、BOLL） 

#### MA-移动平均线

```js
  /**
   * 
   * @param {number[]} close - K线收盘价数组
   * @param {number} n - 均线周期值
   * @return {number[]} 均线数据数组
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
   * @return { dif: number[], dea: number[], bar: number[] } MACD数据对象
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
   * @return { upper: number[], mid: number[], lower: number[] } BOLL数据对象
   */
  indicator.boll(close, size = 20, times = 2)
```

### http - 远程接口请求对象 (支持GET和POST请求)

#### GET请求

```js
  /**
   * 
   * @param {string} url - 目标数据接口地址
   * @return {Promise} 
   */
  http.get(url)
```

#### POST请求

```js
  /**
   * 
   * @param {string} url - 目标数据接口地址
   * @param {object|string} data - 提交的数据信息 (可选)
   * @param {function} callback - 请求成功时执行的回调函数 (可选)
   * @param {string} dataType - 预期的服务器响应的数据类型 (可选 包括xml、json、script 或 html)
   * @return {Promise} 
   */
  http.post(url, data, callback, dataType)
```

示例

```js
  const rsp1 = await http.get('https://example.com/')
  const rsp2 = await http.post('https://example.com/', { test: 'test' })
```

### webhook - webhook通知对象 (支持通知到企业微信)

```js
  /**
   * 
   * @param {string} url - 目标webhook接口地址 (默认已支持企业微信webhook地址)
   * @param {object|string} data - 推送的数据信息
   * @return {void} 
   */
  webhook.notification(data, url)
```

注意: 使用此接口请务必确保已安装壹缠插件 详见 (https://one-quant.com/plugin/壹缠浏览器插件安装说明.pdf)

### joinquant - 聚宽本地数据对象 (拉取各周期K线)

聚宽目前支持沪深市场和国内期货市场(期货主力合约以9999标识和具体月份合约区分) 证券代码格式说明如下:

|交易市场|代码后缀|示例代码|证券简称|
| :----: | :----: | :----: | :----: |
|上海证券交易所|.XSHG|600519.XSHG|贵州茅台|
|深圳证券交易所|.XSHE|000001.XSHE|平安银行|
|中金所|.CCFX|IC9999.CCFX|中证500主力合约|
|大商所|.XDCE|A9999.XDCE|豆一主力合约|
|上期所|.XSGE|AU9999.XSGE|黄金主力合约|
|郑商所|.XZCE|CY8888.XZCE|棉纱期货指数|
|上海国际能源期货交易所|.XINE|SC9999.XINE|原油主力合约|

#### 设置聚宽账户信息

```js
  /**
   * 
   * @param {string} mobile - 聚宽账户
   * @param {string} password - 聚宽密码
   * @return {void} 
   */
  joinquant.setAccount(mobile, password) 
```

#### 拉取标的K线数据

```js
  /**
   * 
   * @param {string} symbol - 聚宽标的代码 (格式说明: 上交所 xxxxxx.XSHG 深交所 xxxxxx.XSHE)
   * @param {string} level - K线周期 (min1、min5、min15、min30、min60、min120、day、week、month)
   * @param {number} length - 拉取的K线数量 (默认1024 最大值5000)
   * @return {Promise<Kline[]>} K线对象数组 (Kline对象格式 { day: 'YYYY-MM-DD HH:mm:ss' | 'YYYY-MM-DD', open: xxxx.xx, high: xxxx.xx, low: xxxx.xx, close: xxxx.xx, volume: xxxxxxx })
   */
  joinquant.captureKline(symbol, level, length = 1024)
```

示例 (获取近30交易日上证指数日K线数据)

```js
  joinquant.setAccount('聚宽账户', '聚宽密码')
  const klines = await joinquant.captureKline('000001.XSHG', 'day', 30)
```

#### 拉取标的特定时间段历史K线数据

当level是week或month时，第一条数据是开始时间date所在的周或月的行情。当level为分钟时，第一条数据是开始时间date所在的一个level切片的行情。 最大获取1000个交易日数据

```js
  /**
   * 
   * @param {string} symbol - 聚宽标的代码 (格式说明: 上交所 xxxxxx.XSHG 深交所 xxxxxx.XSHE)
   * @param {string} level - K线周期 (min1、min5、min15、min30、min60、min120、day、week、month)
   * @param {string} sdate - 起始时间 YYYY-MM-DD HH:mm:ss
   * @param {string} edate - 截止时间 YYYY-MM-DD HH:mm:ss 
   * @return {Promise<Kline[]>} K线对象数组 (Kline对象格式 { day: 'YYYY-MM-DD HH:mm:ss' | 'YYYY-MM-DD', open: xxxx.xx, high: xxxx.xx, low: xxxx.xx, close: xxxx.xx, volume: xxxxxxx })
   */
  joinquant.captureHistroyKline(symbol, level, sdate, edate)
```

示例 (获取 2020-07-01 ~ 2020-07-30 交易日上证指数日K线数据)

```js
  joinquant.setAccount('聚宽账户', '聚宽密码')
  const klines = await joinquant.captureHistroyKline('000001.XSHG', 'day', '2020-07-01 00:00:00', '2020-07-30 00:00:00')
```

使用聚宽接口请务必确保已安装壹缠插件 详见 (https://one-quant.com/plugin/壹缠浏览器插件安装说明.pdf)

### toast - UI提示信息对象

![image](https://user-images.githubusercontent.com/2844717/125153007-76800980-e183-11eb-8773-788c05a889ad.png)

UI提示信息默认展示在图表正上方位置, 多个提示同时出现会自上而下依序展示。

#### 消息提示

```js
  /**
   * 
   * @param {string} text - 消息提示信息
   * @return {void} 
   */
  toast.info(text) 
```

#### 警告提示

```js
  /**
   * 
   * @param {string} text - 消息提示信息
   * @return {void} 
   */
  toast.warn(text) 
```

#### 错误提示

```js
  /**
   * 
   * @param {string} text - 消息提示信息
   * @return {void} 
   */
  toast.error(text) 
```

### log - DevTools调试信息对象

浏览器DevTools(F12快捷键 -> Console) 调试信息接口，用于在脚本开发过程中打印执行信息、确定错误异常代码位置. 

![image](https://user-images.githubusercontent.com/2844717/125153212-c4e1d800-e184-11eb-9340-d73ec9062eaf.png)

#### 普通日志信息

```js
  /**
   * 
   * @param {object|string} msg - 日志打印信息
   * @return {void} 
   */
  log.info(msg) 
```

#### 警告日志信息

```js
  /**
   * 
   * @param {object|string} msg - 日志打印信息
   * @return {void} 
   */
  log.warn(msg) 
```

#### 错误日志信息

```js
  /**
   * 
   * @param {object|string} msg - 日志打印信息
   * @return {void} 
   */
  log.errr(msg) 
```

注意: 频繁打印日志信息 可能会影响浏览器性能 脚本开发完成后 请屏蔽不需要的日志打印信息
