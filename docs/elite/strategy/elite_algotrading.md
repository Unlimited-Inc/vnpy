# 算法委托执行交易

AlgoTrading是用于**算法委托执行交易**的模块，用户可以通过其UI界面操作来便捷完成启动算法、保存配置、停止算法等任务。


## 主要优势

算法交易负责委托订单的具体执行过程。目前，AlgoTrading提供了多种示例算法，用户可以通过把大笔订单自动拆分成合适的小单分批委托，有效降低交易成本和冲击成本，也可以在设定的阈值内进行高抛低吸操作。此外，模块还可监控指定路径的CSV文件进行智能扫单。


## 启动模块

对于用户搭建的算法，需要放到vnpy_algotrading.algos目录中，才能被识别加载。

AlgoTrading模块需要启动之前通过【策略应用】标签页加载。

启动登录VeighNa Elite Trader后，启动模块之前，请先连接交易接口。看到VeighNa Elite Trader主界面【日志】栏输出“合约信息查询成功”之后再启动模块。

请注意，IB接口因为登录时无法自动获取所有的合约信息，只有在用户手动订阅行情时才能获取。因此需要在主界面上先行手动订阅合约行情，再启动模块。

成功连接交易接口后，在菜单栏中点击【功能】-> 【多进程算法交易】，或者点击左侧按钮栏的图标：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/algotrading/0.png)

即可进入多进程算法交易模块的UI界面，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/algotrading/1.png)


## 配置算法

配置参数要求如下：

- 算法：在下拉框中选择要执行的交易算法；
- 交易所：在下拉框中选择交易所；
- 代码：格式为symbol（合约代码）；
- 方向：多、空；
- 开平：开、平、平今、平昨；
- 价格：委托下单的价格；
- 委托数量：委托的总数量；
- 接口：在下拉框中选择要发出委托的接口。


## 启动算法

目前VeighNa一共提供了五种常用的示例算法。本文档以时间加权平均算法（TWAP）为例，介绍算法启动过程。

在图形界面左上角填写好算法配置后，点击【启动算法】按钮即可启动算法，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/algotrading/3.png)

若启动成功，则可在右上角【执行中】界面观测到该算法的执行状态。并在右下角【日志】界面看到“算法启动”日志输出。

图中算法执行的任务具体为：使用时间加权平均算法，买入20手沪深300股指期货2409合约（IF2409），执行价格为3422元，执行时间为120秒，每轮间隔为6秒；即每隔6秒钟，当合约卖一价小于等于3422时，以3422的价格买入1手沪深300股指期货2409合约，将买入操作分割成20次。


## CSV监控

当有较多算法需要启动时，可以通过监控CSV文件来启动。点击图形界面左侧的【CSV监控】按钮，在弹出的对话框中选定要监控的文件夹即可监控路径下新写入的CSV文件，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/algotrading/7.png)

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/algotrading/8.png)

请注意，CSV文件的格式应如下图所示，和左侧编辑区的各字段一致：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/algotrading/9.png)

启动成功后，CSV文件中所有算法的执行情况均会在【执行中】、【已结束】和【日志】界面下显示，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/algotrading/10.png)


## 暂停算法

当用户需要暂停正在执行的交易算法时，可以在【执行中】界面点击【暂停】按钮，暂停某个正在执行的算法交易，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/algotrading/4.png)


## 恢复算法

当用户需要恢复已经暂停的交易算法时，可以在【执行中】界面点击【恢复】按钮，恢复某个已经暂停的算法交易，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/algotrading/5.png)


## 停止算法

当用户需要停止正在执行的交易算法时，可以在【执行中】界面点击【停止】按钮，停止某个正在执行的算法交易，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/algo_trading/6.png)

用户也可以在委托交易界面点击最下方的【全部停止】按钮，一键停止所有执行中的算法交易。


## 数据监控

数据监控界面由三个部分构成：

执行中组件：显示正在执行的算法交易，包括：算法、参数和状态。成功启动算法之后，切换到右上角【执行中】界面，会显示该算法的执行状态。

已结束组件：显示已完成的算法交易，同样包括：算法、参数和状态。算法结束或者停止之后，切换到右上角【已结束】界面，会显示该算法的执行状态。

日志组件：显示启动、停止、完成算法的相关日志信息。


## 示例算法

示例算法路径位于vnpy_algotrading.algos文件夹下（请注意，个别算法是没有写开平方向的，若有需要，可基于自身需求进行个性化修改）。目前，算法交易模块提供了以下五种内置算法：

### TWAP - 时间加权平均算法

时间加权平均算法（TWAP）具体执行步骤如下：

- 将委托数量平均分布在某个时间区域内，每隔一段时间用指定的价格挂出买单（或者卖单）。

- 买入情况：卖一价低于目标价格时，发出委托，委托数量在剩余委托量与委托分割量中取最小值。

- 卖出情况：买一价高于目标价格时，发出委托，委托数量在剩余委托量与委托分割量中取最小值。

### Iceberg - 冰山算法

冰山算法（Iceberg）具体执行步骤如下：

- 在某个价位挂单，但是只挂一部分，直到全部成交。

- 买入情况：先检查撤单，最新Tick卖一价低于目标价格，执行撤单；若无活动委托，发出委托，委托数量在剩余委托量与挂出委托量中取最小值。

- 卖出情况：先检查撤单，最新Tick买一价高于目标价格，执行撤单；若无活动委托，发出委托，委托数量在剩余委托量与挂出委托量中取最小值。

### Sniper - 狙击手算法

狙击手算法（Sniper）具体执行步骤如下：

- 监控最新Tick推送的行情，发现好的价格立刻报价成交。

- 买入情况：最新Tick卖一价低于目标价格时，发出委托，委托数量在剩余委托量与卖一量中取最小值。

- 卖出情况：最新Tick买一价高于目标价格时，发出委托，委托数量在剩余委托量与买一量中取最小值。

### Stop - 条件委托算法

条件委托算法（Stop）具体执行步骤如下：

- 监控最新Tick推送的行情，发现行情突破立刻报价成交。

- 买入情况：Tick最新价高于目标价格时，发出委托，委托价为目标价格加上超价。

- 卖出情况：Tick最新价低于目标价格时，发出委托，委托价为目标价格减去超价。

### BestLimit - 最优限价算法

最优限价算法（BestLimit）具体执行步骤如下：

- 监控最新Tick推送的行情，发现好的价格立刻报价成交。

- 买入情况：先检查撤单：最新Tick买一价不等于目标价格时，执行撤单；若无活动委托，发出委托，委托价格为最新Tick买一价，委托数量为剩余委托量。

- 卖出情况：先检查撤单：最新Tick买一价不等于目标价格时，执行撤单；若无活动委托，发出委托，委托价格为最新Tick卖一价，委托数量为剩余委托量。