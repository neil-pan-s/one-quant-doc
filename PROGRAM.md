
# 量化策略脚本编程指南

![framework](https://user-images.githubusercontent.com/2844717/125124322-7a337200-e12a-11eb-8028-d2dc58dc3ec9.png)

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
   * @param {string} level - K线周期 (min1、min5、min15、min30、min60、min120、min240、day、week、month)
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

![image](https://user-images.githubusercontent.com/2844717/125153212-c4e1d800-e184-11eb-9340-d73ec9062eaf.png)

浏览器DevTools(F12快捷键 -> Console) 调试信息接口，用于在脚本开发过程中打印执行信息、确定错误异常代码位置. 

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

## 传参数据对象

### ohlcv - 当前标的TICK数据
### klines - 当前标的周期K线数据
### twist - 壹缠数据对象 (笔段中枢走势数据)
### echartsCustomSeries - 图表绘图配置数组 (参见 https://echarts.apache.org/zh/option.html#series)
