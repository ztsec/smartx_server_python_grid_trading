# 网格交易策略说明

与[客户端网格交易策略](https://github.com/ztsec/smartx_client_python_grid_trading)相同，区别在于此示例使用服务端API进行开发

具体API文档可以参考[此处](https://smarttest.ztqft.com/sdkDoc/kungfu/2.1.0/guide/howTo_develop_kungfu_strategy.html)

## 一、参数含义解释

### 1. 账号

运行策略所使用的资金账号

### 2. 股票代码
策略标的股票的代码

### 3. 交易市场
与股票代码对应的交易市场（沪市、深市）

### 4. 基准价格
策略初次启动时，用于第一笔交易的基准价格；若委托单全部成交或部分成交后，用成交均价来修改基准价格，作为下一次委托的新的基准价格。

### 5. 卖出价差(%)
实时行情价格与基准价格之间的价差百分比阈值，计算公式为（实时行情价格-基准价格）/基准价格，若超过设定的阈值，则执行卖出交易。

### 6. 买入价差(%)
基准价格与实时行情价格之间的价差百分比阈值，计算公式为（基准价格 - 实时行情价格）/基准价格，若超过设定的阈值，则执行买入交易。

### 7.价格上限
策略运行时，委托价的价格上限。若实时行情价格超过价格上限，策略不再交易直到实时行情价格重新回到价格上限与价格下限之间。

### 8.价格下限
策略运行时，委托价的价格下限。若实时行情价格低于价格下限，策略不再交易直到实时行情价格重新回到价格上限与价格下限之间。

### 9.单次委托数量
策略运行时，单次委托的数量。值得注意的是：若跳空高走或者低走且涨跌幅度达到了设定的买入价差或者卖出价差的整倍数，那么下单委托量也会变为整数倍的单次委托数量。

### 10.最大买入数量
策略单次运行不中断情况下，买入数量的阈值（股）。如果买入数量超过该阈值，则策略不会再进行买入交易。若策略停止后再重新启动策略，上次策略运行买入数量会清零。

### 11.最大卖出数量
策略单次运行不中断情况下，卖出数量的阈值（股）。如果卖出数量超过该阈值，则策略不会再进行卖出交易。若策略停止后再重新启动策略，上次策略运行卖出数量会清零。

### 12.最大轧差
策略单次运行不中断情况下，买入数量与卖出数量之差的绝对值的阈值。如果超过该阈值，若买入数量少，策略只会进行买入交易；若卖出数量少，则策略只会进行卖出交易。 

## 二、策略逻辑
1. 首先，用户要在 excel 配置文件中对各参数进行配置；
2. 策略开始运行后，根据用户的配置，接收与标的股票对应的行情数据；
3. 在收到行情数据的基础上，根据实时行情价格和基准价格，判断满足交易条件。如果当前价格能够触发策略交易，在满足“价格上限”、“价格下限”、“最大买入数量”、“最大卖出数量”、“最大轧差”等约束条件的前提下，进行相应买入或卖出交易；
4. 策略会计算最新完成的委托单的成交均价来修改对应标的股票的基准价格，并同时把该价格存放到 excel 配置文件中对应标的股票的基准价格，便于下次启动策略继续使用该基准价格。