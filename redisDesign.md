# Redis表设计


## 单一Key值redis 数据

||||||
|---|---|---|---|---|
|表名|redis数据结构类型|主键|数据结构|说明|
|t_user|hash|investorId:counterId|CTTSUser|用户表根据投资者和柜台号区分|
|t_user_session|hash|investorId:clientToken|UserSessionField|用户会话表根据用户名和设备号来区分，一个会话一条记录|
|local_order_id|key|-|int|保存报单号，递增|
|ctp_requestid_counter|key|-|int|保存ctp柜台请求号，递增|
|t_investor_fund|hash|investorId:brokerId|CUnityRspInvestorAccountField|用户资金表根据投资者和经纪商代码区分|
|t_investor_position|hash|investorId:brokerId|CUnityRspInvestorPositionField|用户持仓表根据投资者和经纪商代码区分|
|t_market_data|hash|instrumentId|CCTPMarketDataField|柜台行情信息根据合约代码区分|


## 多Key值redis数据
### 即为结构化业务表分散到多个redis key上
||||||
|---|---|---|---|---|
|t_user_order:investorId:brokerId:tradingday|hash|investorId:brokerId:instrumentId:orderSysId:exchangeId|CUnityOrderField|订单表根据用户号经纪商号和日期为key建立hash表，表中field为订单的联合主键|
|t_user_trade:investorId:brokerId:tradingday|hash|investorId:brokerId:instrumentId:orderSysId:exchangeId|CUnityTradeField|成交表根据用户号经纪商号和日期为key建立hash表，表中field为成交订单的联合主键|

