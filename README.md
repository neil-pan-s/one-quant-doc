
# 缠中说禅 - 缠论技术分析

聚焦于多级别K线走势分析 实时自动非同级别分解 笔段画线、中枢标识 递归分析整体走势 作为买卖分析参考

地址: <https://one-quant.com/#/twist> (默认标的为沪深300指数 搜索框可检索标的)

![demo](./doc/demo.png)

![流程](./doc/流程.png)

https://user-images.githubusercontent.com/2844717/121382541-4a514d00-c979-11eb-82c6-68780817041b.mp4

## 支持标的

- 沪深证券 (免费)
- 沪深可转债 (免费)
- 港股&美股
- 国内期货
- 国际期货
- 加密货币

## VIP & SVIP服务

- VIP服务
  - 5s 15s 30s 秒级别K线 逐秒观察市场 🔥
  - 逐K行情回放 分析复盘利器 👍
  - 全市场行情 支持加密货币、期货和港美股
- SVIP服务
  - 实时标注缠论三类买卖点 🔥
  - 缠论买卖点选股器 支持沪深市场股票、国内期货 👍
  - 支持Webhook推送买卖点通知 实时推送企业微信
  - 支持多周期看盘 竖屏三周期、横屏四周期

## 工具使用说明

1. **笔段走势** - 黄线为当前级别K线构成的笔，橙线为基于当前级别笔特征序列生成的段，紫线为基于当前级别段生成的走势
2. **MACD面积** - 笔段的末端数字为对应笔段的MACD面积，MACD面积为当前笔段的红绿面积之和
3. **中枢级别** - 中枢以白色、橙色和紫色框标注，两端数字为中枢上下沿值
4. **K线回放** - 快捷键说明: 空格键(开始/暂停/继续 K线回放)、左箭头 (向前回退一根K线)、右箭头(向后前进一根K线)
5. **图表操作** - 支持滚轮缩放图表、鼠标拖拽移动图表
6. **标的搜索** - 点击搜索图标 可搜索全市场标的 点击返回的搜索结果切换标的
7. **秒级K线** - 交易所不直接提供秒级别K线数据 所以无秒级别历史K线 依据毫秒Tick合并生成实时高精度秒级K线（需在开盘时才能使用）

## 多级别联立

多级别联立 判断走势情况 找寻更多的合适买卖点和形态特征

![cube](./doc/cube-x.jpg)

## 买卖定式

![买卖定式](./doc/买卖定式.jpg)

## 缠论量化思路

1. [如何严格画出缠论中的笔](https://mp.weixin.qq.com/s?__biz=MzUzMzY0MTc4OQ==&mid=2247484364&idx=1&sn=2a155608d1a12704b813059442c24ff6&chksm=faa1ac9ecdd625884eb6270748081062b4df53874cde1d997cf5e7c50af43669f8aaa3359e4a&scene=178&cur_album_id=1494635016360919043#rd)
2. [如何量化缠中说缠的线段](https://mp.weixin.qq.com/s?__biz=MzUzMzY0MTc4OQ==&mid=2247484850&idx=1&sn=f734307260f28d1684b54a016ddb6da3&chksm=faa1aae0cdd623f69ec7f2c8033ca8ddb5706e505551cb78816ddf086bfd920505f172da771d&scene=178&cur_album_id=1494635016360919043#rd)
3. [如何量化缠中说禅中枢](https://mp.weixin.qq.com/s?__biz=MzUzMzY0MTc4OQ==&mid=2247484542&idx=1&sn=d2f7fdb66b96e976a6e7e998651f8728&chksm=faa1ab2ccdd6223ac4b1e4ef681dc1f6217298589cc33e6d5abf04299e91975a38a48638e7dc&scene=178&cur_album_id=1494635016360919043#rd)
4. [如何理解缠论的同级别分解](https://mp.weixin.qq.com/s?__biz=MzUzMzY0MTc4OQ==&mid=2247484616&idx=1&sn=a0b7a8487ebc4853a0117254b41b7c9a&chksm=faa1ab9acdd6228c2c795f06c9c3407c903c16547e40484f88ef3d312101176a5acd90b63b68&scene=178&cur_album_id=1494635016360919043#rd)

相关量化思路由李大硬提供， 详见公众号（缠中说硬）

## 反馈讨论 & Bug提交

<https://github.com/neil-pan-s/twist-quant/issues>

## 量化交流 & 7x24财经快讯推送群

![微信](./doc/neil0pan0s.png "缠论交流&量化交流") ![壹电报](./doc/finance.jpg "加入7x24财经快讯群 尽览全球实时财经快讯")

