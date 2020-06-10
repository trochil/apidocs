---
title: 蜂鸟API

language_tabs:
  - python

toc_footers:
  - <a href='http://www.trochil.cn/'>蜂鸟首页</a>

includes:
  - errors

search: true
---

# 简介

蜂鸟数据是轻量级金融终端，提供主流金融市场的实时报价和历史数据。我们提供高度统一和强大的REST API和Websocket API，让您轻松获取金融数据。

登录[蜂鸟数据](http://www.trochil.com)，注册成为我们的会员，即可免费使用API服务，每天可免费调用250次REST API，并试用7天Websocket，适用于所有接口和市场。

覆盖市场和交易所：

* A股：上海证券交易所和深圳证券交易所的全部A股
* 港股：香港证券交易所上市的股票(不包括牛熊证)
* 美股：纽约证券交易所和纳斯达克交易所
* 期货：郑州商品交易所，大量商品交易所，上海期货交易所，中国金融期货交易所
* 数字货币：币安(Binance), Okex, 火币(Huobi), Coinbase, Poloniex, Bitfinex
* 外汇：80多个货币对，包含新兴市场国家的汇率

**http请求base url:**

api.trochil.cn

# API

所有接口均要求认证，注册成为我们的用户后可以在个人主页找到API key，请求数据时传递您的API key作为参数，详情看代码实例。

# 行情数据

## A股

### 产品列表

> 示例代码：查询A股产品列表


```python
import requests

info = requests.get('http://api.trochil.cn/v1/cnstock/markets',
                    params={'apikey': 'your apikey'})
```


> 返回结果：查询A股产品列表

```json
{
    'timestamp': 1583475638869,
    'data': [
        {'symbol': 'SH600000', 'name': '浦发银行', 'exchange': '上海证券交易所'},
        {'symbol': 'SH600004', 'name': '白云机场', 'exchange': '上海证券交易所'},
        ...
    ],
    'status': 'ok'
}
```

获取上海证券交易所和深证证券交易所上市交易的全部A股。

#### HTTP请求

`GET v1/cnstock/markets`

#### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
apikey | String | True | NA | 用户申请的apikey

#### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | list | 对应A股的名称和隶属交易所信息
status | String | 请求结果状态

### 实时报价

> 示例代码：查询A股实时报价

```python
import requests

info = requests.get('http://api.trochil.cn/v1/cnstock/quote',
                    params={'symbol': 'SZ300564,SZ300820',
                            'apikey': 'your apikey'})
```

> 返回结果：查询A股实时报价

```json
{
    'timestamp': 1590752105706,
    'data': {
        'SH600004': {'symbol': 'SH600004', 'name': '白云机场', 'last': '16.35', 'pre_close': '15.6', 'open': '15.45',
                     'volume': '27333100.0', 'volume_lot': '273331.0', 'volume_cny': '444270000.0', 'bid1': '16.34',
                     'bid1_qty': '434', 'bid2': '16.33', 'bid2_qty': '2814', 'bid3': '16.32', 'bid3_qty': '88',
                     'bid4': '16.31', 'bid4_qty': '323', 'bid5': '16.3', 'bid5_qty': '882', 'ask1': '16.35',
                     'ask1_qty': '98', 'ask2': '16.36', 'ask2_qty': '599', 'ask3': '16.37', 'ask3_qty': '164',
                     'ask4': '16.38', 'ask4_qty': '378', 'ask5': '16.39', 'ask5_qty': '153', 'change': '0.75',
                     'percent_change': '4.81', 'high': '16.59', 'low': '15.42', 'pe': '48.79', 'pb': '2.04',
                     'trade_market_value': '338.33', 'total_market_value': '338.33', 'datetime': '2020-05-29 15:40:50',
                     'timestamp': '1590738050000'},
        'SH600000': {'symbol': 'SH600000', 'name': '浦发银行', 'last': '10.57', 'pre_close': '10.53', 'open': '10.45',
                     'volume': '30659200.0', 'volume_lot': '306592.0', 'volume_cny': '322830000.0', 'bid1': '10.55',
                     'bid1_qty': '51', 'bid2': '10.54', 'bid2_qty': '186', 'bid3': '10.53', 'bid3_qty': '2587',
                     'bid4': '10.52', 'bid4_qty': '4044', 'bid5': '10.51', 'bid5_qty': '4766', 'ask1': '10.57',
                     'ask1_qty': '1494', 'ask2': '10.58', 'ask2_qty': '5354', 'ask3': '10.59', 'ask3_qty': '3282',
                     'ask4': '10.6', 'ask4_qty': '4682', 'ask5': '10.61', 'ask5_qty': '1300', 'change': '0.04',
                     'percent_change': '0.38', 'high': '10.57', 'low': '10.45', 'pe': '5.19', 'pb': '0.61',
                     'trade_market_value': '2970.57', 'total_market_value': '3102.51',
                     'datetime': '2020-05-29 15:40:39',
                     'timestamp': '1590738039000'}},
    'status': 'ok'
}
```

查询沪深A股的实时报价(level1)，每次最多查询25个产品。上海证券交易所股票代码加前缀'SH'，深圳证券交易所股票代码加前缀'SZ'。

#### HTTP请求

`GET v1/cnstock/quote`

#### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 股票代码，同时获取多个股票以逗号分隔，如SZ300817,SZ300820
apikey | String | True | NA | 用户申请的apikey

#### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | dict | 对应个股品种的报价信息
status | String | 请求结果状态

#### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
symbol | String | 股票代码
name | String | 股票中文名称
last | String | 最新成交价
pre_close | String | 昨日收盘价
open | String | 当日开盘价
volume | String | 当日成交量(股)
volume_lot | String | 当日成交量(手)
volume_cny | String | 当日成交金额
bid1 | String | 最新买一价格
bid1_qty | String | 最新买一挂单量
bid2 | String | 最新买二价格
bid2_qty | String | 最新买二挂单量
bid3 | String | 最新买三价格
bid3_qty | String | 最新买三挂单量
bid4 | String | 最新买四价格
bid4_qty | String | 最新买四挂单量
bid5 | String | 最新买五价格
bid5_qty | String | 最新买五挂单量
ask1 | String | 最新卖一价格
ask1_qty | String | 最新卖一挂单量
ask2 | String | 最新卖二价格
ask2_qty | String | 最新卖二挂单量
ask3 | String | 最新卖三价格
ask3_qty | String | 最新卖三挂单量
ask4 | String | 最新卖四价格
ask4_qty | String | 最新卖四挂单量
ask5 | String | 最新卖五价格
ask5_qty | String | 最新卖五挂单量
change | String | 涨跌数额
percent_change | String | 涨跌百分比
high | String | 当日最高价
low | String | 当日最低价
pe | String | 市盈率
pb | String | 市净率
trade_market_value | String | 流动市值
total_market_value | String | 总市值
datetime | String | 最新成交北京时间
timestamp | String | 最新成交时间戳

### 日图历史

> 示例代码：查询A股日图历史

```python
import requests

info = requests.get('http://api.trochil.cn/v1/cnstock/history',
                    params={
                        'symbol': 'SH600012',
                        'start_date': '2020-03-01',
                        'end_date': '2020-03-04',
                        'apikey': 'your apikey'}
                    )
```

> 返回结果：查询A股日图历史

```json
{
    'timestamp': 1583477561417,
    'data': [
        {'datetime': '2020-03-02', 'open': 5.2, 'high': 5.32, 'low': 5.2, 'close': 5.3, 'volume': 29389.0,
         'symbol': 'SH600012'},
        {'datetime': '2020-03-03', 'open': 5.34, 'high': 5.39, 'low': 5.3, 'close': 5.34, 'volume': 36059.0,
         'symbol': 'SH600012'},
        {'datetime': '2020-03-04', 'open': 5.31, 'high': 5.57, 'low': 5.31, 'close': 5.54, 'volume': 103134.0,
         'symbol': 'SH600012'}],
    'status': 'ok'
}
```

获取沪深A股的日图历史数据。

#### HTTP请求

`GET v1/cnstock/history`

#### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 股票代码,如SH600012
start_date | String | False | 数据起始时间 | 样本起始时间,如2019-01-01
end_date | String | False | 最近更新时间 | 样本结束时间,如2019-09-09
freq | String | False | daily | 频率
sort | String | False | asc | 按时间戳排序，asc(升序), desc(降序)
apikey | String | True | NA | 用户申请的apikey

#### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | list | 对应个股品种的报价信息
status | String | 请求结果状态

#### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
datetime | String | k线时间
open | Float | 开盘价
high | Float | 最高价
low | Float | 最低价
close | Float | 收盘价
volume | Float | 成交量
symbol | String | 股票代码

### 多产品单日历史

> 示例代码：查询A股多股票单日数据

```python
import requests

info = requests.get('http://api.trochil.cn/v1/cnstock/history_one_day',
                    params={
                        'symbol': 'SH600012,SH600015',
                        'date': '2011-09-29',
                        'apikey': 'your apikey'}
                    )
```

> 返回结果：查询A股多股票单日数据

```json
{
    'timestamp': 1583477976901,
    'data': [
        {'datetime': '2011-09-29', 'open': 3.413, 'high': 3.421, 'low': 3.351, 'close': 3.366, 'volume': 17783.0, 'symbol': 'SH600012'},
        {'datetime': '2011-09-29', 'open': 4.144, 'high': 4.254, 'low': 4.139, 'close': 4.207, 'volume': 195176.0, 'symbol': 'SH600015'}],
    'status': 'ok'
}
```

同时获取多个股票单天的K线数据，最多同时查询25个。

#### HTTP请求

`GET v1/cnstock/history_one_day`

#### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 股票代码,如SH600012,多个股票品种以逗号分隔,逗号之间不要有空格
date | String | True | NA | 数据获取时间,如2019-01-01
sort | String | False | asc | 按股票代码的字符顺序排序，asc(升序)，desc(降序)
apikey | String | True | NA | 用户申请的apikey

#### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | list | 对应股票品种的报价信息
status | String | 请求结果状态

#### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
datetime | String | k线时间
open | Float | 开盘价
high | Float | 最高价
low | Float | 最低价
close | Float | 收盘价
volume | Float | 成交量
symbol | String | 个股品种代码

### 日内数据

> 示例代码：查询A股日内数据

```python
import requests

info = requests.get('http://api.trochil.cn/v1/cnstock/intraday',
                    params={
                        'symbol': 'SZ300817',
                        'timeframe': '60',
                        'range': 2,
                        'apikey': 'your apikey'}
                    )
print(info.json())
```

> 返回结果：查询A股日内数据

```json
{
    'timestamp': 1583478112575,
    'data': [
    {'datetime': '2020-03-05 09:30:00', 'open': 41.5, 'high': 42.99, 'low': 39.0, 'close': 41.5, 'volume': 64686.0, 'symbol': 'SZ300817'},
    {'datetime': '2020-03-05 10:30:00', 'open': 41.5, 'high': 41.5, 'low': 40.0, 'close': 40.36, 'volume': 8865.0, 'symbol': 'SZ300817'},
    {'datetime': '2020-03-05 13:00:00', 'open': 40.36, 'high': 41.83, 'low': 39.8, 'close': 41.33, 'volume': 12049.0, 'symbol': 'SZ300817'},
    {'datetime': '2020-03-05 14:00:00', 'open': 41.33, 'high': 41.62, 'low': 40.04, 'close': 40.2, 'volume': 16964.0, 'symbol': 'SZ300817'}],
    'status': 'ok'
}
```

获取沪深A股的日内K线，1分钟图最多返回过去7天的数据，5分钟图和60分钟图最多返回过去30天的数据。沪深A股暂时不提供1分钟历史数据。

#### HTTP请求

`GET v1/cnstock/intraday`

#### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 股票代码，如SZ300817
timeframe | int | True | NA | 频率，5分钟为5，60分钟为60,目前仅支持5分钟和60分钟
range | int | True | 7 | 查询天数, 7代表7天, 范围1-30
endTime | String | False | 当前时间 | 请求的结束时间
apikey | String | True | NA | 用户申请的apikey

#### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | list | 对应股票品种的报价信息
status | String | 请求结果状态

#### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
datetime | String | k线时间
open | Float | 开盘价
high | Float | 最高价
low | Float | 最低价
close | Float | 收盘价
volume | Float | 成交量
symbol | String | 个股品种代码


## 港股

### 产品列表

> 实例代码：查询港股产品列表:

```python
import requests

info = requests.get('http://api.trochil.cn/v1/hkstock/markets',
                    params={'apikey': 'your apikey'})
```

> 返回示例：查询港股产品列表:

```json
{
    'timestamp': 1583478480228,
    'data': [
        {'symbol': 'HK00001', 'name': '长和', 'exchange': '香港证券交易所'},
        {'symbol': 'HK00002', 'name': '中电控股', 'exchange': '香港证券交易所'},
        ...
    ],
    'status': 'ok'
}
```

获取香港证券交易所上市的全部股票(不包含ETH和牛熊证)。

#### HTTP请求

`GET v1/hkstock/markets`

#### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
apikey | String | True | NA | 用户申请的apikey

#### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | list | 对应港股的名称和隶属交易所信息
status | String | 请求结果状态

### 实时报价

> 示例代码：查询港股实时报价

```python
import requests

info = requests.get('http://api.trochil.cn/v1/hkstock/quote',
                    params={
                        'symbol': 'HK09938,HK09968',
                        'apikey': 'your apikey'}
                    )
```

> 示例代码：查询港股实时报价

```json
{
    'timestamp': 1590041611265,
    'data': {
        'HK09938': {"symbol": "HK09938", "name": "华和控股", "last": "0.245", "change": "0.012", "percent_change": "5.15",
                    "high": "0.255", "low": "0.223", "open": "0.233", "pre_close": "0.233", "pe": "7.38",
                    "52week_high": "2.79", "52week_low": "0.2", "volume": "11770000.0", "volume_hkd": "2876975.0",
                    "datetime": "2020-05-21 14:10:07", "timestamp": "1590041407000"},
        'HK09968': {"symbol": "HK09968", "name": "汇景控股", "last": "2.07", "change": "0.02", "percent_change": "0.98",
                    "high": "2.09", "low": "2.03", "open": "2.03", "pre_close": "2.05", "pe": "16.11",
                    "52week_high": "2.2", "52week_low": "1.73", "volume": "2040000.0", "volume_hkd": "4208520.0",
                    "datetime": "2020-05-21 14:12:47", "timestamp": "1590041567000"}},
    'status': 'ok'
}
```

查询港股的实时报价，不包含bid/ask价格，每次最多查询25个产品。股票代码加前缀'HK'。

#### HTTP请求

`GET v1/hkstock/quote`

#### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 股票代码，同时获取多个股票以逗号分隔，如HK09938,HK09968
apikey | String | True | NA | 用户申请的apikey

#### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | dict | 对应个股品种的报价信息
status | String | 请求结果状态

#### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
symbol | String | 股票代码
name | String | 股票中文名称
last | String | 最新成交价
change | String | 涨跌变动数额
percent_change | String | 涨跌变动比率
high | String | 当日最高价
low | String | 当日最低价
open | String | 当日开盘价
pre_close | String | 昨日收盘价
pe | String | 市盈率
52week_high | String | 52周最高价
52week_low | String | 52周最低价
volume | String | 股票成交量
volume_hkd | String | 成交金额
datetime | String | 最新成交北京时间
timestamp | String | 最新成交时间戳

### 日图历史

> 示例代码：查询港股日图历史

```python
import requests

info = requests.get('http://api.trochil.cn/v1/hkstock/history',
                    params={
                        'symbol': 'HK09968',
                        'start_date': '2020-02-01',
                        'end_date': '2020-02-04',
                        'apikey': 'your apikey'}
                    )
```

> 返回结果：查询港股日图历史

```json
{
    'timestamp': 1583478838781,
    'data': [
        {'datetime': '2020-02-03', 'open': 1.92, 'high': 1.95, 'low': 1.9, 'close': 1.95, 'volume': 1780000.0, 'symbol': 'HK09968'},
        {'datetime': '2020-02-04', 'open': 1.94, 'high': 2.12, 'low': 1.94, 'close': 1.97, 'volume': 2790000.0, 'symbol': 'HK09968'}],
    'status': 'ok'
}
```

获取港股的日图历史数据。

#### HTTP请求

`GET v1/hkstock/history`

#### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 股票代码,如HK09968
start_date | String | False | 数据起始时间 | 样本起始时间,如2019-01-01
end_date | String | False | 最近更新时间 | 样本结束时间,如2019-09-09
freq | String | False | daily | 频率
sort | String | False | asc | 按时间戳排序，asc(升序), desc(降序)
apikey | String | True | NA | 用户申请的apikey

#### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | list | 对应个股品种的报价信息
status | String | 请求结果状态

#### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
datetime | String | k线时间
open | Float | 开盘价
high | Float | 最高价
low | Float | 最低价
close | Float | 收盘价
volume | Float | 成交量
symbol | String | 股票代码

### 多产品单日历史

> 代码示例：查询港股多股票的单日数据

```python
import requests

info = requests.get('http://api.trochil.cn/v1/hkstock/history_one_day',
                    params={
                        'symbol': 'HK09938,HK09968',
                        'date': '2020-02-04',
                        'apikey': 'your apikey'}
                    )
```

> 代码示例：查询港股多股票单日数据

```json
{
    'timestamp': 1583479171828,
    'data': [
        {'datetime': '2020-02-04', 'open': 0.69, 'high': 0.69, 'low': 0.66, 'close': 0.67, 'volume': 680000.0, 'symbol': 'HK09938'},
        {'datetime': '2020-02-04', 'open': 1.94, 'high': 2.12, 'low': 1.94, 'close': 1.97, 'volume': 2790000.0, 'symbol': 'HK09968'}],
    'status': 'ok'
}
```

同时获取多个股票单天的K线数据，最多同时查询25个。

#### HTTP请求

`GET v1/hkstock/history_one_day`

#### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 股票代码,如HK09938,多个股票品种以逗号分隔,逗号之间不要有空格
date | String | True | NA | 数据获取时间,如2019-01-01
sort | String | False | asc | 按股票代码的字符顺序排序，asc(升序)，desc(降序)
apikey | String | True | NA | 用户申请的apikey

#### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | list | 对应股票品种的报价信息
status | String | 请求结果状态

#### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
datetime | String | k线时间
open | Float | 开盘价
high | Float | 最高价
low | Float | 最低价
close | Float | 收盘价
volume | Float | 成交量
symbol | String | 个股品种代码
