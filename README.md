
# 缠中说禅 - 缠论技术分析

聚焦于多级别K线走势分析 基于1minK线为小级别 递归分析整体走势 作为买卖分析参考

![demo](./img/demo.jpg)

演示地址: <http://quant.neil-pan.com/twist/> (演示版本为sh000016静态行情数据)

## 笔、段和中枢

1. 黄色为1分笔，橙色为基于1分笔特征序列生成的1分段，紫色为基于1分段特征序列生成的更大级别段
2. 段的末端数字为对应段的MACD面积，上涨段计算MACD红色面积(正数), 下跌段计算MACD绿色面积（负数）
3. 中枢以橙色和紫色框标注，两端数字为上下沿值

<!-- 
## 使用说明

标的代码: 交易所标识(sh为上海交易所 sz为深圳交易所) + 证券标的代码

```txt
  sh000001 为 上证指数 
  sz000001 为 平安银行
```

1. 下载项目到本地
2. 浏览器打开访问 dist/index.html#/?code=标的代码 (如sh000001)
3. 默认支持每30s实时更新行情，结合开盘时间自动开始更新K线 
-->

## 缠论量化思路

1. [如何严格画出缠论中的笔](https://mp.weixin.qq.com/s?__biz=MzUzMzY0MTc4OQ==&mid=2247484364&idx=1&sn=2a155608d1a12704b813059442c24ff6&chksm=faa1ac9ecdd625884eb6270748081062b4df53874cde1d997cf5e7c50af43669f8aaa3359e4a&scene=178&cur_album_id=1494635016360919043#rd)
2. [如何量化缠中说缠的线段](https://mp.weixin.qq.com/s?__biz=MzUzMzY0MTc4OQ==&mid=2247484850&idx=1&sn=f734307260f28d1684b54a016ddb6da3&chksm=faa1aae0cdd623f69ec7f2c8033ca8ddb5706e505551cb78816ddf086bfd920505f172da771d&scene=178&cur_album_id=1494635016360919043#rd)
3. [如何量化缠中说禅中枢](https://mp.weixin.qq.com/s?__biz=MzUzMzY0MTc4OQ==&mid=2247484542&idx=1&sn=d2f7fdb66b96e976a6e7e998651f8728&chksm=faa1ab2ccdd6223ac4b1e4ef681dc1f6217298589cc33e6d5abf04299e91975a38a48638e7dc&scene=178&cur_album_id=1494635016360919043#rd)
4. [如何理解缠论的同级别分解](https://mp.weixin.qq.com/s?__biz=MzUzMzY0MTc4OQ==&mid=2247484616&idx=1&sn=a0b7a8487ebc4853a0117254b41b7c9a&chksm=faa1ab9acdd6228c2c795f06c9c3407c903c16547e40484f88ef3d312101176a5acd90b63b68&scene=178&cur_album_id=1494635016360919043#rd)

见doc目录下相关公众号文档, 量化思路由李大硬提供， 详见公众号（缠中说硬）

## 反馈讨论 & Bug提交

<https://github.com/neil-pan-s/twist-quant/issues>

## 缠论交流 & 量化交流

![微信](./img/wx.jpg "neil-pan-s")