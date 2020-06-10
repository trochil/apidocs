---
title: 蜂鸟API

language_tabs:
  - python

toc_footers:
  - <a href='http://www.trochil.com/'>蜂鸟首页</a>

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

# A股

## 产品列表

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

### HTTP请求

`GET v1/cnstock/markets`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
apikey | String | True | NA | 用户申请的apikey

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | list | 对应A股的名称和隶属交易所信息
status | String | 请求结果状态

## 实时报价

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

### HTTP请求

`GET v1/cnstock/quote`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 股票代码，同时获取多个股票以逗号分隔，如SZ300817,SZ300820
apikey | String | True | NA | 用户申请的apikey

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | dict | 对应个股品种的报价信息
status | String | 请求结果状态

### data说明

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

## 日图历史

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

### HTTP请求

`GET v1/cnstock/history`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 股票代码,如SH600012
start_date | String | False | 数据起始时间 | 样本起始时间,如2019-01-01
end_date | String | False | 最近更新时间 | 样本结束时间,如2019-09-09
freq | String | False | daily | 频率
sort | String | False | asc | 按时间戳排序，asc(升序), desc(降序)
apikey | String | True | NA | 用户申请的apikey

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | list | 对应个股品种的报价信息
status | String | 请求结果状态

### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
datetime | String | k线时间
open | Float | 开盘价
high | Float | 最高价
low | Float | 最低价
close | Float | 收盘价
volume | Float | 成交量
symbol | String | 股票代码

## 多产品单日历史

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

### HTTP请求

`GET v1/cnstock/history_one_day`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 股票代码,如SH600012,多个股票品种以逗号分隔,逗号之间不要有空格
date | String | True | NA | 数据获取时间,如2019-01-01
sort | String | False | asc | 按股票代码的字符顺序排序，asc(升序)，desc(降序)
apikey | String | True | NA | 用户申请的apikey

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | list | 对应股票品种的报价信息
status | String | 请求结果状态

### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
datetime | String | k线时间
open | Float | 开盘价
high | Float | 最高价
low | Float | 最低价
close | Float | 收盘价
volume | Float | 成交量
symbol | String | 个股品种代码

## 日内数据

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

### HTTP请求

`GET v1/cnstock/intraday`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 股票代码，如SZ300817
timeframe | int | True | NA | 频率，5分钟为5，60分钟为60,目前仅支持5分钟和60分钟
range | int | True | 7 | 查询天数, 7代表7天, 范围1-30
endTime | String | False | 当前时间 | 请求的结束时间
apikey | String | True | NA | 用户申请的apikey

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | list | 对应股票品种的报价信息
status | String | 请求结果状态

### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
datetime | String | k线时间
open | Float | 开盘价
high | Float | 最高价
low | Float | 最低价
close | Float | 收盘价
volume | Float | 成交量
symbol | String | 个股品种代码


# 港股

## 产品列表

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

### HTTP请求

`GET v1/hkstock/markets`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
apikey | String | True | NA | 用户申请的apikey

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | list | 对应港股的名称和隶属交易所信息
status | String | 请求结果状态

## 实时报价

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

### HTTP请求

`GET v1/hkstock/quote`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 股票代码，同时获取多个股票以逗号分隔，如HK09938,HK09968
apikey | String | True | NA | 用户申请的apikey

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | dict | 对应个股品种的报价信息
status | String | 请求结果状态

### data说明

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

## 日图历史

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

### HTTP请求

`GET v1/hkstock/history`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 股票代码,如HK09968
start_date | String | False | 数据起始时间 | 样本起始时间,如2019-01-01
end_date | String | False | 最近更新时间 | 样本结束时间,如2019-09-09
freq | String | False | daily | 频率
sort | String | False | asc | 按时间戳排序，asc(升序), desc(降序)
apikey | String | True | NA | 用户申请的apikey

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | list | 对应个股品种的报价信息
status | String | 请求结果状态

### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
datetime | String | k线时间
open | Float | 开盘价
high | Float | 最高价
low | Float | 最低价
close | Float | 收盘价
volume | Float | 成交量
symbol | String | 股票代码

## 多产品单日历史

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

### HTTP请求

`GET v1/hkstock/history_one_day`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 股票代码,如HK09938,多个股票品种以逗号分隔,逗号之间不要有空格
date | String | True | NA | 数据获取时间,如2019-01-01
sort | String | False | asc | 按股票代码的字符顺序排序，asc(升序)，desc(降序)
apikey | String | True | NA | 用户申请的apikey

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | list | 对应股票品种的报价信息
status | String | 请求结果状态

### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
datetime | String | k线时间
open | Float | 开盘价
high | Float | 最高价
low | Float | 最低价
close | Float | 收盘价
volume | Float | 成交量
symbol | String | 个股品种代码


# 美股

## 产品列表

> 代码示例：查询美股产品列表

```python
import requests

info = requests.get('http://api.trochil.cn/v1/usstock/markets',
                    params={'apikey': 'your apikey'})
```

> 返回结果：查询美股产品列表

```json
{
    'timestamp': 1583480134214,
    'data': [
        {'symbol': 'AAPL', 'name': '苹果公司', 'exchange': '纳斯达克交易所'},
        {'symbol': 'MSFT', 'name': '微软公司', 'exchange': '纳斯达克交易所'},
        ...
    ],
    'status': 'ok'
}
```

获取纽约证券交易所和纳斯达克交易所上市交易的全部股票。

### HTTP请求

`GET v1/usstock/markets`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
apikey | String | True | NA | 用户申请的apikey

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | list | 对应美股的名称和隶属交易所信息
status | String | 请求结果状态

## 实时报价

> 示例代码：查询美股实时报价

```python
import requests

info = requests.get('http://api.trochil.cn/v1/usstock/quote',
                    params={'symbol': 'AAPL,BABA,BF.B', 'apikey': 'your apikey'})
```

> 返回结果：查询美股实时报价

```json
{
    'timestamp': 1590559857542,
    'data': {
        'BABA': {'symbol': 'BABA', 'bid': 201.71, 'ask': 201.82, 'bid_qty': 23, 'ask_qty': 51,
                 'timestamp': '1590523199990', 'datetime': '2020-05-27 03:59:59', 'name': '阿里巴巴', 'change': 2.02,
                 'percent_change': 1.01, 'pre_close': 199.7, 'open': 205.94, 'high': 206.8, 'low': 201.0,
                 'volume': 28683134.0, 'mktcap': 541279611116.0, 'pe': 21.5743308, 'exchange': 'NYSE'},
        'AAPL': {'symbol': 'AAPL', 'bid': 316.72, 'ask': 316.84, 'bid_qty': 52, 'ask_qty': 19,
                 'timestamp': '1590523199964', 'datetime': '2020-05-27 03:59:59', 'name': '苹果公司', 'change': -2.16,
                 'percent_change': -0.68, 'pre_close': 318.89, 'open': 323.5, 'high': 324.24, 'low': 316.5,
                 'volume': 31380454.0, 'mktcap': 1373173460731.0, 'pe': 24.62908394, 'exchange': 'NASDAQ'},
        'BF.B': {'symbol': 'BF.B', 'bid': 63.54, 'ask': 63.55, 'bid_qty': 61, 'ask_qty': 4,
                 'timestamp': '1590523199982', 'datetime': '2020-05-27 03:59:59', 'name': '布朗霍文集团', 'change': -1.24,
                 'percent_change': -1.91, 'pre_close': 64.78, 'open': 66.2, 'high': 66.2, 'low': 63.35,
                 'volume': 1582838.0, 'mktcap': 30513647377.0, 'pe': 35.30000144, 'exchange': 'NYSE'}
    },
    'status': 'ok'
}

```

查询美股的实时报价(level1)，每次最多查询25个产品。

### HTTP请求

`GET v1/usstock/quote`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 股票代码，同时获取多个股票以逗号分隔，如AAPL,GOOG,MSFT
apikey | String | True | NA | 用户申请的apikey

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | dict | 对应个股品种的报价信息
status | String | 请求结果状态

### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
symbol | String | 美股对应代码
bid | float | 最新最优买价
ask | float | 最新最优卖价
bid_qty | float | 最新最优买价对应的挂单量
ask_qty | float | 最新最优卖价对应的挂单量
timestamp | String | 最新交易发生时间
datetime | String | 最新交易发生北京时间
name | String | 美股中文名称
change | float | 价格变动数额
percent_change | float | 价格变动百分比
pre_close | float | 昨日收盘价
open | float | 今日开盘价
high | float | 今日最高价
low | float | 今日最低价
volume | float | 成交量
mktcap | float | 市值
pe | float | 市盈率
exchange | String | 交易所

## 日图历史

> 代码示例：查询美股日图历史

```python
import requests

info = requests.get('http://api.trochil.cn/v1/usstock/history',
                    params={
                        'symbol': 'BABA',
                        'start_date': '2020-02-01',
                        'end_date': '2020-02-05',
                        'apikey': 'your apikey'}
                    )
```

> 返回结果：查询美股日图历史

```json
{
    'timestamp': 1583480946631,
    'data': [
        {'datetime': '2020-02-03', 'open': 208.67, 'high': 215.02, 'low': 208.67, 'close': 213.1, 'volume': 14131887.0, 'symbol': 'BABA'},
        {'datetime': '2020-02-04', 'open': 221.35, 'high': 224.38, 'low': 220.49, 'close': 222.88, 'volume': 16695141.0, 'symbol': 'BABA'},
        {'datetime': '2020-02-05', 'open': 226.52, 'high': 226.7, 'low': 217.54, 'close': 220.22, 'volume': 15766067.0, 'symbol': 'BABA'}],
    'status': 'ok'
}
```

获取美股的日图历史数据。

### HTTP请求

`GET v1/usstock/history`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 股票代码,如BABA
start_date | String | False | 数据起始时间 | 样本起始时间,如2019-01-01
end_date | String | False | 最近更新时间 | 样本结束时间,如2019-09-09
freq | String | False | daily | 频率
sort | String | False | asc | 按时间戳排序，asc(升序), desc(降序)
apikey | String | True | NA | 用户申请的apikey

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | list | 对应个股的报价信息
status | String | 请求结果状态

### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
datetime | String | k线时间
open | Float | 开盘价
high | Float | 最高价
low | Float | 最低价
close | Float | 收盘价
volume | Float | 成交量
symbol | String | 股票代码

## 多产品单日历史

> 示例代码：查询美股多股票单日数据

```python
import requests

info = requests.get('http://api.trochil.cn/v1/usstock/history_one_day',
                    params={
                        'symbol': 'BABA,JD',
                        'date': '2020-02-04',
                        'apikey': 'your apikey'}
                    )
```

> 返回结果：查询美股多股票单日数据

```json
{
    'timestamp': 1583481081616,
    'data': [
        {'datetime': '2020-02-04', 'open': 221.35, 'high': 224.38, 'low': 220.49, 'close': 222.88, 'volume': 16695141.0, 'symbol': 'BABA'},
        {'datetime': '2020-02-04', 'open': 40.31, 'high': 41.28, 'low': 40.07, 'close': 40.35, 'volume': 16286551.0, 'symbol': 'JD'}],
    'status': 'ok'
}
```

同时获取多个股票单天的K线数据，最多同时查询25个。

### HTTP请求

`GET v1/usstock/history_one_day`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 股票代码,如BABA,多个股票品种以逗号分隔,逗号之间不要有空格
date | String | True | NA | 数据获取时间,如2019-01-01
sort | String | False | asc | 按股票代码的字符顺序排序，asc(升序)，desc(降序)
apikey | String | True | NA | 用户申请的apikey

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | list | 对应美股品种的报价信息
status | String | 请求结果状态

### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
datetime | String | k线时间
open | Float | 开盘价
high | Float | 最高价
low | Float | 最低价
close | Float | 收盘价
volume | Float | 成交量
symbol | String | 美股品种代码

## 日内数据

> 代码示例：查询美股日内数据

```python
import requests

info = requests.get('http://api.trochil.cn/v1/usstock/intraday',
                    params={
                        'symbol': 'BABA',
                        'timeframe': '60',
                        'range': 2,
                        'apikey': 'your apikey'}
                    )
```

> 返回结果：查询美股日内数据

```json
{
    'timestamp': 1583481191233,
    'data': [
        {'datetime': '2020-03-05 09:30:00', 'open': 210.0, 'high': 212.29, 'low': 209.14, 'close': 211.47, 'volume': 3120157.0, 'symbol': 'BABA'},
        {'datetime': '2020-03-05 10:30:00', 'open': 211.4, 'high': 215.15, 'low': 211.08, 'close': 214.23, 'volume': 2429986.0, 'symbol': 'BABA'},
        {'datetime': '2020-03-05 11:30:00', 'open': 214.23, 'high': 215.0, 'low': 213.01, 'close': 213.45, 'volume': 1770316.0, 'symbol': 'BABA'},
        {'datetime': '2020-03-05 12:30:00', 'open': 213.41, 'high': 213.55, 'low': 211.16, 'close': 212.21, 'volume': 1218989.0, 'symbol': 'BABA'},
        {'datetime': '2020-03-05 13:30:00', 'open': 212.21, 'high': 212.42, 'low': 210.52, 'close': 210.84, 'volume': 1037356.0, 'symbol': 'BABA'},
        {'datetime': '2020-03-05 14:30:00', 'open': 210.84, 'high': 211.56, 'low': 209.95, 'close': 210.38, 'volume': 1553312.0, 'symbol': 'BABA'},
        {'datetime': '2020-03-05 15:30:00', 'open': 210.38, 'high': 212.04, 'low': 210.33, 'close': 211.44, 'volume': 1477890.0, 'symbol': 'BABA'}],
    'status': 'ok'
}
```

获取美股的日内K线，支持1分钟，5分钟和60分钟K线。1分钟图最多返回过去7天的数据，5分钟图和60分钟图最多返回过去30天的数据。

### HTTP请求

`GET v1/usstock/intraday`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 股票代码,如BABA
timeframe | int | True | NA | 频率，1分钟为1,5分钟为5，60分钟为60
range | int | True | NA | 查询天数, 7代表7天, 范围1-30,1分钟仅支持7日以内,<br>5分钟60分钟支持30日以内
endTime | String | NA | 当前时间 | 请求的结束时间
apikey | String | True | NA | 用户申请的apikey

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | list | 对应股票品种的报价信息
status | String | 请求结果状态

### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
datetime | String | k线时间
open | Float | 开盘价
high | Float | 最高价
low | Float | 最低价
close | Float | 收盘价
volume | Float | 成交量
symbol | String | 股票品种代码


# 期货

## 产品列表

> 示例代码：查询期货产品列表

```python
import requests

info = requests.get('http://api.trochil.cn/v1/cnfuture/markets',
                    params={'apikey': 'your apikey'})
```

> 示例代码：查询期货产品列表

```json
{
    'timestamp': 1584587390538,
    'data': [
        {'symbol': 'IC2004', 'name': '中证500指数2004', 'exchange': '中国金融期货交易所'},
        {'symbol': 'IC2006', 'name': '中证500指数2006', 'exchange': '中国金融期货交易所'},
        {'symbol': 'IC2009', 'name': '中证500指数2009', 'exchange': '中国金融期货交易所'},
        {'symbol': 'IC0', 'name': '中证500指数连续', 'exchange': '中国金融期货交易所'},
        {'symbol': 'IC2003', 'name': '中证500指数2003', 'exchange': '中国金融期货交易所'},
        {'symbol': 'IF2004', 'name': '沪深300指数2004', 'exchange': '中国金融期货交易所'}],
    'status': 'ok'
}
```

获取国内四大期货交易所的全部期货品种，主力合约和可交易合约。

### HTTP请求

`GET v1/cnfuture/markets`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
apikey | String | True | NA | 用户申请的apikey

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | list | 对应合约品种的名称和隶属交易所信息
status | String | 请求结果状态

## 实时报价

> 示例代码：查询期货合约的实时报价

```python
import requests

info = requests.get('http://api.trochil.cn/v1/cnfuture/quote',
                    params={
                        'symbol': 'V0,V2007',
                        'apikey': 'your apikey'}
                    )
```

> 示例代码：查询期货合约的实时报价

```json
{
    'timestamp': 1590752847016,
    'data': {
        'V0': {'name': 'PVC连续', 'open': '6035.0', 'high': '6085.0', 'low': '5995.0', 'pre_close': '6040.0',
               'bid': '6040.0', 'ask': '6050.0', 'last': '6040.0', 'settle': '0.0', 'pre_settle': '6020.0',
               'bid_qty': '102.0', 'ask_qty': '257.0', 'open_interest': '198264.0', 'volume': '146221.0',
               'datetime': '2020-05-29 15:00:00', 'timestamp': '1590735600000', 'symbol': 'V0', 'change': '20.0',
               'percent_change': '0.33'},
        'V2007': {'name': 'PVC2007', 'open': '6045.0', 'high': '6145.0', 'low': '6040.0', 'pre_close': '6125.0',
                  'bid': '6070.0', 'ask': '6120.0', 'last': '6125.0', 'settle': '0.0', 'pre_settle': '6040.0',
                  'bid_qty': '2.0', 'ask_qty': '1.0', 'open_interest': '2065.0', 'volume': '4912.0',
                  'datetime': '2020-05-29 15:00:00', 'timestamp': '1590735600000', 'symbol': 'V2007', 'change': '85.0',
                  'percent_change': '1.41'}},
    'status': 'ok'
}

```

查询期货合约的实时报价(level1)，郑商所，大商所，上期所返回实时行情；中金所行情延迟15分钟，不包含bid,ask数据。

### HTTP请求

`GET v1/cnfuture/quote`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 产品合约，同时获取多个合约以逗号分隔，如AU2006,AU2008
apikey | String | True | NA | 用户申请的apikey

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | dict | 对应合约品种的报价信息
status | String | 请求结果状态

### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
name | String | 标准合约名称
open | String | 今日开盘价
high | String | 今日最高价
low | String | 今日最低价
pre_close | String | 前一日收盘价
bid | String | 最优买价
ask | String | 最优卖价
last | String | 最新成交价
settle | String | 结算价
pre_settle | String | 前一日结算价
bid_qty | String | 最优买价数量
ask_qty | String | 最优卖价数量
open_interest | String | 持仓量
volume | String | 成交量
datetime | String | 最新成交北京时间
timestamp | String | 最新成交时间戳
symbol | String | 合约代码
change | String | 涨跌变动金额
percent_change | String | 涨跌变动比率


## 日图历史

> 示例代码：查询期货合约日图历史

```python
import requests

info = requests.get('http://api.trochil.cn/v1/cnfuture/history',
                    params={
                        'symbol': 'V0',
                        'freq': 'weekly',
                        'start_date': '2019-11-12',
                        'end_date': '2020-01-01',
                        'apikey': 'your apikey'}
                    )
```

> 返回结果：查询期货合约日图历史

```json
{
    'timestamp': 1579485233076,
    'data': [
        {'open': 6485.0, 'high': 6670.0, 'low': 6480.0, 'close': 6610.0, 'volume': 894842, 'position': 349929.5,
         'datetime': '2019-11-17'},
        {'open': 6615.0, 'high': 6620.0, 'low': 6525.0, 'close': 6595.0, 'volume': 736518, 'position': 367483.2,
         'datetime': '2019-11-24'},
        {'open': 6600.0, 'high': 6905.0, 'low': 6595.0, 'close': 6875.0, 'volume': 1835134, 'position': 409573.2,
         'datetime': '2019-12-01'},
        {'open': 6880.0, 'high': 6930.0, 'low': 6660.0, 'close': 6715.0, 'volume': 1614508, 'position': 348538.8,
         'datetime': '2019-12-08'},
        {'open': 6725.0, 'high': 6800.0, 'low': 6505.0, 'close': 6560.0, 'volume': 1181394, 'position': 296074.4,
         'datetime': '2019-12-15'},
        {'open': 6555.0, 'high': 6630.0, 'low': 6525.0, 'close': 6605.0, 'volume': 980858, 'position': 426604.8,
         'datetime': '2019-12-22'},
        {'open': 6605.0, 'high': 6610.0, 'low': 6465.0, 'close': 6505.0, 'volume': 898110, 'position': 444499.2,
         'datetime': '2019-12-29'},
        {'open': 6505.0, 'high': 6545.0, 'low': 6465.0, 'close': 6525.0, 'volume': 382154, 'position': 467070.0,
         'datetime': '2020-01-05'}
    ]
}
```

获取期货合约的日图历史数据。

### HTTP请求

`GET v1/cnfuture/history`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 产品合约,如AU2006
start_date | String | False | 数据起始时间 | 样本起始时间,如2019-01-01
end_date | String | False | 最近更新时间 | 样本结束时间,如2019-09-09
freq | String | False | daily | 频率
sort | String | False | asc | 按时间戳排序，asc(升序), desc(降序)
apikey | String | True | NA | 用户申请的apikey

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | list | 对应合约品种的报价信息
status | String | 请求结果状态

### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
datetime | String | k线时间
open | Float | 开盘价
high | Float | 最高价
low | Float | 最低价
close | Float | 收盘价
volume | Float | 成交量
symbol | String | 合约代码

## 多产品单日历史

> 示例代码：查询多份期货合约的单日数据

```python
import requests

info = requests.get('http://api.trochil.cn/v1/cnfuture/history_one_day',
                    params={
                        'symbol': 'V0,V2002,V2003,V2004,V2005',
                        'date': '2019-12-25',
                        'apikey': 'your apikey'}
                    )
```

> 返回结果：查询多份期货合约的单日数据

```json
{
    'timestamp': 1579485233101,
    'data': [
        {'datetime': '2019-12-25', 'open': 6535.0, 'high': 6535.0, 'low': 6480.0, 'close': 6495.0, 'volume': 140420,
         'position': 443094, 'symbol': 'V0'},
        {'datetime': '2019-12-25', 'open': 6475.0, 'high': 6475.0, 'low': 6475.0, 'close': 6475.0, 'volume': 2,
         'position': 366, 'symbol': 'V2002'},
        {'datetime': '2019-12-25', 'open': 6535.0, 'high': 6535.0, 'low': 6480.0, 'close': 6495.0, 'volume': 140420,
         'position': 443094, 'symbol': 'V2005'}
    ]
}
```

同时获取多个期货合约单天的K线数据，最多同时查询25个。

### HTTP请求

`GET v1/cnfuture/history_one_day`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 期货合约,如AU2006,多个合约品种以逗号分隔
date | String | True | NA | 数据获取时间,如2019-01-01
sort | String | False | asc | 按股票代码的字符顺序排序，asc(升序)，desc(降序)
apikey | String | True | NA | 用户申请的apikey

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | list | 对应合约品种的报价信息
status | String | 请求结果状态

### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
datetime | String | k线时间
open | Float | 开盘价
high | Float | 最高价
low | Float | 最低价
close | Float | 收盘价
volume | Float | 成交量
symbol | String | 合约品种代码

## 日内数据

> 代码示例：查询期货日内数据

```python
import requests

info = requests.get('http://api.trochil.cn/v1/cnfuture/intraday',
                    params={
                        'symbol': 'V0',
                        'timeframe': '30',
                        'range': 7,
                        'apikey': 'your apikey'}
                    )
```

> 返回结果：查询期货日内数据

```json
{
    'timestamp': 1579485233129,
    'data': [
        {'datetime': '2020-01-17 10:00:00', 'open': 6470.0, 'high': 6475.0, 'low': 6470.0, 'close': 6470.0,
         'volume': 73, 'position': 206712, 'symbol': 'V0'},
        {'datetime': '2020-01-17 10:30:00', 'open': 6465.0, 'high': 6470.0, 'low': 6465.0, 'close': 6465.0,
         'volume': 90, 'position': 206077, 'symbol': 'V0'},
        {'datetime': '2020-01-17 11:00:00', 'open': 6470.0, 'high': 6470.0, 'low': 6465.0, 'close': 6465.0, 'volume': 8,
         'position': 205800, 'symbol': 'V0'}
    ]
}
```

获取国内期货合约的日内K线，支持5分钟和60分钟K线，最多返回过去30天的数据。

### HTTP请求

`GET v1/cnfuture/intraday`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 产品合约,如AU2006
timeframe | int | True | NA | 频率，5分钟为5，60分钟为60
range | int | True | NA | 查询天数, 7代表7天, 范围1-30
endTime | String | NA | 当前时间 | 请求的结束时间
apikey | String | True | NA | 用户申请的apikey

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | list | 对应合约品种的报价信息
status | String | 请求结果状态

### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
datetime | String | k线时间
open | Float | 开盘价
high | Float | 最高价
low | Float | 最低价
close | Float | 收盘价
volume | Float | 成交量
symbol | String | 合约品种代码


# 数字货币

## 产品列表

> 代码示例：查询数字货币产品列表

```python
import requests

info = requests.get('http://api.trochil.cn/v1/crypto/markets', params={'apikey': 'your apikey'})
```

> 返回结果：查询数字货币产品列表

```json
{
    'timestamp': 1590483547229,
    'data': [
        {'symbol': 'ETHBTC', 'exchange': 'BINANCE'}, {'symbol': 'LTCBTC', 'exchange': 'BINANCE'},
        {'symbol': 'XMRUSDT', 'exchange': 'OKEX'}, {'symbol': 'XLMUSDT', 'exchange': 'OKEX'},
        {'symbol': 'IOSTUSDT', 'exchange': 'OKEX'}, {'symbol': 'THETAUSDT', 'exchange': 'OKEX'},
        {'symbol': 'XLMBTC', 'exchange': 'HUOBI'}, {'symbol': 'ONTBTC', 'exchange': 'HUOBI'},
        {'symbol': 'BSVBTC', 'exchange': 'HUOBI'},...
    ],
    'status': 'ok'
}
```

获取部分主流交易所的大部分相对活跃的交易对信息。

### HTTP请求

`GET v1/crypto/markets`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
apikey | String | True | NA | 用户申请的apikey

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | list | 对应数字货币的名称和隶属交易所信息
status | String | 请求结果状态

### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
symbol | String | 交易对代码
exchange | String | 交易对所支持的交易所

## 实时报价

> 示例代码：查询数字货币实时报价

```python
import requests

info = requests.get('http://api.trochil.cn/v1/crypto/quote',
                    params={'symbol': 'binance.algousdt,binance.btcusdt', 'apikey': 'your apikey'})
```

> 返回结果：查询数字货币实时报价

```json
{
    'timestamp': 1590753183765,
    'data': {
        'BINANCE.BTCUSDT': {'exchange': 'BINANCE', 'symbol': 'BTCUSDT', 'last': '9385.2', 'bid': '9385.19',
                            'bid_qty': '3.351073', 'ask': '9386.26', 'ask_qty': '0.230002', 'open': '9264.99',
                            'high': '9625.47', 'low': '9251.65', 'pre_close': '9264.99', 'base_volume': '86081.880102',
                            'quote_volume': '814481628.0267937', 'change': '120.21', 'percent_change': '1.297',
                            'timestamp': '1590753183523'},
        'HUOBI.BTCUSDT': {'exchange': 'HUOBI', 'symbol': 'BTCUSDT', 'bid': '9387.0', 'bid_qty': '0.008430004400940485',
                          'ask': '9387.01', 'ask_qty': '3.6117433271904473', 'timestamp': '1590753183315',
                          'last': '9387.01', 'open': '9260.24', 'high': '9620.0', 'low': '9250.0',
                          'pre_close': '9260.24', 'change': '126.77', 'percent_change': '1.37',
                          'base_volume': '53793.03817115715', 'quote_volume': '508953040.5793978'}},
    'status': 'ok'
}

```

查询数字货币的实时报价(level1)，每次最多查询25个产品。

### HTTP请求

`GET v1/crypto/quote`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 交易对代码，同时获取多个以逗号分隔，如binance.algousdt,binance.btcusdt<br>拼接格式为交易所名称.交易对,具体交易所支持哪些交易对请<br>查询v1/crypto/markets接口
apikey | String | True | NA | 用户申请的apikey

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | dict | 对应数字货币品种的报价信息
status | String | 请求结果状态

### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
exchange | String | 交易所名称
symbol | String | 交易对代码
last | String | 最新成交价
bid | String | 最新最优买价
bid_qty | String | 最优买价对应的挂单量
ask | String | 最新最优卖价
ask_qty | String | 最优卖价对应的挂单量
open | String | 今日开盘价
high | String | 今日最高价
low | String | 今日最低价
pre_close | String | 昨日收盘价
base_volume | String | 交易量基于交易币
quote_volume | String | 交易量基于基础币
change | String | 价格变动数额
percent_change | String | 价格变动百分比
timestamp | String | 最新交易发生时间

## 日图历史

> 代码示例：查询数字货币日图历史

```python
import requests

info = requests.get('http://api.trochil.cn/v1/crypto/history',
                    params={'symbol': 'binance.btcusdt', 'start_date': '2020-03-01', 'end_date': '2020-03-04', 'apikey': 'your apikey'})
```

> 返回结果：查询数字货币日图历史

```json
{
    'timestamp': 1590484543499,
    'data': [
        {'datetime': '2020-03-01', 'open': 8523.61, 'high': 8750.0, 'low': 8411.0, 'close': 8531.88,
         'volume': 43892.201779, 'symbol': 'btcusdt'},
        {'datetime': '2020-03-02', 'open': 8530.3, 'high': 8965.75, 'low': 8498.0, 'close': 8915.24,
         'volume': 60401.31773, 'symbol': 'btcusdt'},
        {'datetime': '2020-03-03', 'open': 8911.18, 'high': 8919.65, 'low': 8651.0, 'close': 8760.07,
         'volume': 55154.997282, 'symbol': 'btcusdt'},
        {'datetime': '2020-03-04', 'open': 8760.07, 'high': 8848.29, 'low': 8660.0, 'close': 8750.87,
         'volume': 38696.482578, 'symbol': 'btcusdt'}
    ],
    'status': 'ok'
}
```

获取数字货币的日图历史数据,目前仅支持binance交易所对应交易对

### HTTP请求

`GET v1/crypto/history`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 交易对代码如binance.btcusdt
start_date | String | False | 数据起始时间 | 样本起始时间,如2019-01-01
end_date | String | False | 最近更新时间 | 样本结束时间,如2019-09-09
freq | String | False | daily | 频率
sort | String | False | asc | 按时间戳排序，asc(升序), desc(降序)
apikey | String | True | NA | 用户申请的apikey

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | list | 对应数字货币的报价信息
status | String | 请求结果状态

### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
datetime | String | k线时间
open | Float | 开盘价
high | Float | 最高价
low | Float | 最低价
close | Float | 收盘价
volume | Float | 成交量
symbol | String | 交易对代码

## 多交易对单日历史

> 示例代码：查询数字货币多交易对单日数据

```python
import requests

info = requests.get('http://api.trochil.cn/v1/crypto/history_one_day',
                    params={'symbol': 'binance.btcusdt,binance.ethusdt', 'date': '2019-09-29', 'apikey': 'your apikey'})
```

> 返回结果：查询数字货币多交易对单日数据

```json
{
    'timestamp': 1590484797308,
    'data': [
        {'datetime': '2019-09-29', 'open': 8199.38, 'high': 8229.13, 'low': 7890.0, 'close': 8043.82, 'volume': 31544.211388, 'symbol': 'btcusdt'},
        {'datetime': '2019-09-29', 'open': 173.5, 'high': 174.5, 'low': 164.12, 'close': 169.24, 'volume': 410855.12176, 'symbol': 'ethusdt'}
    ],
    'status': 'ok'
}
```

同时获取多个交易对单天的K线数据，最多同时查询25个,目前仅支持binance交易所对应交易对

### HTTP请求

`GET v1/crypto/history_one_day`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 交易对代码,如binance.btcusdt,binance.ethusdt,多个交易对<br>以逗号分隔,逗号之间不要有空格
date | String | True | NA | 数据获取时间,如2019-01-01
sort | String | False | asc | 按交易对代码的字符顺序排序，asc(升序)，desc(降序)
apikey | String | True | NA | 用户申请的apikey

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | list | 对应数字货币交易对的k线信息
status | String | 请求结果状态

### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
datetime | String | k线时间
open | Float | 开盘价
high | Float | 最高价
low | Float | 最低价
close | Float | 收盘价
volume | Float | 成交量
symbol | String | 数字货币交易对代码

## 日内数据

> 代码示例：查询数字货币日内数据

```python
import requests

info = requests.get('http://api.trochil.cn/v1/crypto/intraday',
                    params={'symbol': 'binance.btcusdt', 'timeframe': '60', 'range': 2, 'apikey': 'your apikey'})
```

> 返回结果：查询数字货币日内数据

```json
{
    'timestamp': 1590485108213,
    'data': [
        {'datetime': '2020-05-24 18:00:00', 'open': 8947.84, 'high': 8982.14, 'low': 8890.41, 'close': 8941.99,
         'volume': 2350.063354, 'symbol': 'btcusdt'},
        {'datetime': '2020-05-24 19:00:00', 'open': 8941.04, 'high': 9040.0, 'low': 8937.0, 'close': 9018.45,
         'volume': 3038.765464, 'symbol': 'btcusdt'},
        {'datetime': '2020-05-24 20:00:00', 'open': 9017.37, 'high': 9040.0, 'low': 8983.05, 'close': 9000.31,
         'volume': 2110.613043, 'symbol': 'btcusdt'},
        {'datetime': '2020-05-25 22:00:00', 'open': 8899.42, 'high': 8923.57, 'low': 8865.1, 'close': 8903.3,
         'volume': 1170.047882, 'symbol': 'btcusdt'},
        {'datetime': '2020-05-25 23:00:00', 'open': 8903.24, 'high': 8940.0, 'low': 8889.9, 'close': 8900.35,
         'volume': 1495.507457, 'symbol': 'btcusdt'}
    ],
    'status': 'ok'
}
```

获取数字货币的日内K线，支持1分钟，5分钟和60分钟K线。1分钟图最多返回过去7天的数据，5分钟图和60分钟图最多返回过去30天的数据,目前仅支持binance交易所对应交易对

### HTTP请求

`GET v1/crypto/intraday`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 数字货币交易对代码,如binance.btcusdt
timeframe | int | True | NA | 频率，1分钟为1,5分钟为5，60分钟为60
range | int | True | NA | 查询天数, 7代表7天, 范围1-30,1分钟仅支持7日以内,<br>5分钟60分钟支持30日以内
endTime | String | NA | 当前时间 | 请求的结束时间
apikey | String | True | NA | 用户申请的apikey

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | list | 对应交易对的报价信息
status | String | 请求结果状态

### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
datetime | String | k线时间
open | Float | 开盘价
high | Float | 最高价
low | Float | 最低价
close | Float | 收盘价
volume | Float | 成交量
symbol | String | 交易对代码


# 外汇

## 产品列表

> 代码示例：查询外汇产品列表

```python
import requests

info = requests.get('http://api.trochil.cn/v1/forex/markets', params={'apikey': 'your apikey'})
```

> 返回结果：查询外汇产品列表

```json
{
    'timestamp': 1590487034346,
    'data': [
        {'symbol': 'EURUSD', 'name': '欧元/美元', 'market_type': 'currency', 'exchange': 'OTC'},
        {'symbol': 'GBPUSD', 'name': '英镑/美元', 'market_type': 'currency', 'exchange': 'OTC'},
        {'symbol': 'USDJPY', 'name': '美元/日元', 'market_type': 'currency', 'exchange': 'OTC'},
        ...
        {'symbol': 'USDCAD', 'name': '美元/加元', 'market_type': 'currency', 'exchange': 'OTC'},
    ],
    'status': 'ok'
}
```

获取外汇市场的交易对信息。

### HTTP请求

`GET v1/forex/markets`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
apikey | String | True | NA | 用户申请的apikey

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | list | 对应外汇的名称和隶属交易所信息
status | String | 请求结果状态

### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
symbol | String | 交易对代码
name | String | 外汇中文名称
market_type | String | 产品类型
exchange | String | 交易所

## 实时报价

> 示例代码：查询外汇品种实时报价

```python
import requests

info = requests.get('http://api.trochil.cn/v1/forex/quote',
                    params={'symbol': 'EURUSD,USDJPY,WTICOUSD', 'apikey': 'your apikey'})
```

> 返回结果：查询外汇品种实时报价

```json
{
    'timestamp': 1590753304210,
    'data': {
        'EURUSD': {'symbol': 'EURUSD', 'bid': 1.11375, 'ask': 1.11376, 'timestamp': '1590753303000',
                   'datetime': '2020-05-29 19:55:03', 'last': 1.11375, 'open': 1.10894, 'high': 1.11136, 'low': 1.1088,
                   'pre_close': 1.10896, 'change': 0.00479, 'percent_change': 0.43, 'name': '欧元/美元'},
        'USDJPY': {'symbol': 'USDJPY', 'bid': 107.146, 'ask': 107.147, 'timestamp': '1590753302000',
                   'datetime': '2020-05-29 19:55:02', 'last': 107.146, 'open': 107.393, 'high': 107.393, 'low': 107.044,
                   'pre_close': 107.389, 'change': -0.243, 'percent_change': -0.23, 'name': '美元/日元'}},
    'status': 'ok'
}

```

查询外汇品种的实时报价(level1)，每次最多查询25个产品。

### HTTP请求

`GET v1/forex/quote`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 外汇品种代码，同时获取多个以逗号分隔，如EURUSD,USDJPY,WTICOUSD
apikey | String | True | NA | 用户申请的apikey

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | dict | 对应外汇品种的报价信息
status | String | 请求结果状态

### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
open | float | 今日开盘价
high | float | 今日最高价
low | float | 今日最低价
pre_close | float | 昨日收盘价
change | float | 价格变动数额
percent_change | float | 价格变动百分比
name | String | 外汇中文名称
symbol | String | 外汇代码
bid | float | 最新最优买价
ask | float | 最新最优卖价
timestamp | String | 最新交易发生时间
datetime | String | 最新交易发生北京时间
last | float | 最新成交价

## 日图历史

> 代码示例：查询外汇日图历史

```python
import requests

info = requests.get('http://api.trochil.cn/v1/forex/history',
                    params={'symbol': 'EURUSD', 'start_date': '2020-02-01', 'end_date': '2020-02-05', 'apikey': 'your apikey'})
```

> 返回结果：查询外汇日图历史

```json
{
    'timestamp': 1590487835285,
    'data': [
        {'datetime': '2020-02-02', 'open': 1.10957, 'high': 1.10957, 'low': 1.10822, 'close': 1.10894, 'symbol': 'eurusd'},
        {'datetime': '2020-02-03', 'open': 1.10893, 'high': 1.10893, 'low': 1.10361, 'close': 1.10626, 'symbol': 'eurusd'},
        {'datetime': '2020-02-04', 'open': 1.10627, 'high': 1.10643, 'low': 1.1033, 'close': 1.10449, 'symbol': 'eurusd'},
        {'datetime': '2020-02-05', 'open': 1.1045, 'high': 1.10481, 'low': 1.09939, 'close': 1.10006, 'symbol': 'eurusd'}
    ],
    'status': 'ok'
}
```

获取外汇的日图历史数据。

### HTTP请求

`GET v1/forex/history`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 外汇代码如EURUSD
start_date | String | False | 数据起始时间 | 样本起始时间,如2019-01-01
end_date | String | False | 最近更新时间 | 样本结束时间,如2019-09-09
freq | String | False | daily | 频率
sort | String | False | asc | 按时间戳排序，asc(升序), desc(降序)
apikey | String | True | NA | 用户申请的apikey

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | list | 对应外汇的报价信息
status | String | 请求结果状态

### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
datetime | String | k线时间
open | Float | 开盘价
high | Float | 最高价
low | Float | 最低价
close | Float | 收盘价
symbol | String | 外汇代码

## 多外汇品种单日历史

> 示例代码：查询外汇多品种单日数据

```python
import requests

info = requests.get('http://api.trochil.cn/v1/forex/history_one_day',
                    params={'symbol': 'EURUSD,USDJPY', 'date': '2020-03-12', 'apikey': 'your apikey'})
```

> 返回结果：查询外汇多品种单日数据

```json
{
    'timestamp': 1590488143772,
    'data': [
        {'datetime': '2020-03-12', 'open': 1.12617, 'high': 1.13338, 'low': 1.10551, 'close': 1.11831, 'symbol': 'eurusd'},
        {'datetime': '2020-03-12', 'open': 104.54, 'high': 106.11, 'low': 103.091, 'close': 104.679, 'symbol': 'usdjpy'}
    ],
    'status': 'ok'
}
```

同时获取多个外汇品种单天的K线数据，最多同时查询25个。

### HTTP请求

`GET v1/forex/history_one_day`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 交易对代码,如EURUSD,USDJPY,多个交易对以逗号分隔,逗号之间不要有空格
date | String | True | NA | 数据获取时间,如2019-01-01
sort | String | False | asc | 按交易对代码的字符顺序排序，asc(升序)，desc(降序)
apikey | String | True | NA | 用户申请的apikey

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | list | 对应数字货币交易对的k线信息
status | String | 请求结果状态

### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
datetime | String | k线时间
open | Float | 开盘价
high | Float | 最高价
low | Float | 最低价
close | Float | 收盘价
symbol | String | 外汇品种代码

## 日内数据

> 代码示例：查询外汇日内数据

```python
import requests

info = requests.get('http://api.trochil.cn/v1/forex/intraday',
                    params={'symbol': 'EURUSD', 'timeframe': '5', 'range': 2, 'apikey': 'your apikey'})
```

> 返回结果：查询外汇日内数据

```json
{
    'timestamp': 1590488280601,
    'data': [
        {'datetime': '2020-05-24 21:00:00', 'open': 1.08973, 'high': 1.08973, 'low': 1.08963, 'close': 1.08973, 'symbol': 'eurusd'},
        {'datetime': '2020-05-24 21:05:00', 'open': 1.08982, 'high': 1.09005, 'low': 1.08973, 'close': 1.08973, 'symbol': 'eurusd'},
        ...
        {'datetime': '2020-05-25 23:50:00', 'open': 1.09001, 'high': 1.0901, 'low': 1.08999, 'close': 1.09009, 'symbol': 'eurusd'},
        {'datetime': '2020-05-25 23:55:00', 'open': 1.09008, 'high': 1.09021, 'low': 1.09007, 'close': 1.09019, 'symbol': 'eurusd'}
    ],
    'status': 'ok'
}
```

获取外汇的日内K线，支持1分钟，5分钟和60分钟K线。1分钟图最多返回过去7天的数据，5分钟图和60分钟图最多返回过去30天的数据。

### HTTP请求

`GET v1/forex/intraday`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 外汇品种代码,如EURUSD
timeframe | int | True | NA | 频率，1分钟为1,5分钟为5，60分钟为60
range | int | True | NA | 查询天数, 7代表7天, 范围1-30,1分钟仅支持7日以内,<br>5分钟60分钟支持30日以内
endTime | String | NA | 当前时间 | 请求的结束时间
apikey | String | True | NA | 用户申请的apikey

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
timestamp | int | 请求响应时间戳
data | list | 对应交易对的报价信息
status | String | 请求结果状态

### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
datetime | String | k线时间
open | Float | 开盘价
high | Float | 最高价
low | Float | 最低价
close | Float | 收盘价
symbol | String | 外汇品种代码


# Websocket行情数据

## 简介

### 接入url

websocket接入地址

ws://stream.trochil.cn/ws 后拼接市场类型,如A股为ws://stream.trochil.cn/ws/cnstock

### 订阅主题

成功建立与Websocket服务器的连接后，需要进行apikey认证,发送如下消息认证apikey:

{'op': 'auth', 'apikey': your apiKey}

apikey认证通过后Websocket客户端发送如下请求以订阅特定主题,支持以列表形式一次性发送多个主题：

{'op': 'sub', 'topic': topic to sub}

topic为需要订阅的品种代码如股票代码SH600000,单个订阅可以传字符串 "SH600000",也可以是列表["SH600000"],多个品种以列表传送如["SH600000", "SH600001"],
数字货币需要在交易对前加上交易所名称如["binance.btcusdt"],具体各个市场类型的代码请通过http api请求markets接口获取

成功订阅后，Websocket客户端将收到确认

之后, 一旦所订阅的主题有更新，Websocket客户端将收到服务器推送的更新消息

### 取消订阅

取消订阅的格式如下,取消订阅支持以列表形式一次性取消订阅多个主题：

{'op': 'unsub', 'topic': topic to unsub}

取消订阅成功后服务器发送确认信息

## A股实时数据

### 主题订阅

地址 /cnstock
一旦A股数据产生，Websocket服务器将通过此订阅主题接口推送至客户端

### A股实时数据更新字段列表

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
symbol | String | 股票代码
name | String | 股票中文名称
bid | String | 最优买价
ask | String | 最优卖价
last | String | 最新成交价
bidQty | String | 最优买价数量
askQty | String | 最优卖价数量
timestamp | String | 最新成交时间

## 港股实时数据

### 主题订阅

地址 /hkstock
一旦港股数据产生，Websocket服务器将通过此订阅主题接口推送至客户端

### 港股实时数据更新字段列表

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
symbol | String | 股票代码
name | String | 股票中文名称
last | String | 最新成交价
timestamp | String | 最新成交时间

## 美股实时数据

### 主题订阅

地址 /usstock
一旦美股数据产生，Websocket服务器将通过此订阅主题接口推送至客户端

### 美股实时数据更新字段列表

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
symbol | String | 股票代码
name | String | 股票中文名称
last | String | 最新成交价
timestamp | String | 实时价格时间

## 国内期货实时数据

### 主题订阅

地址 /cnfuture
一旦期货数据产生，Websocket服务器将通过此订阅主题接口推送至客户端

### 期货实时数据更新字段列表

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
symbol | String | 标准合约名称
symbol_cn | String | 标准合约中文
open | String | 今日开盘价
high | String | 今日最高价
low | String | 今日最低价
preClose | String | 前一日收盘价
bid | String | 最优买价
ask | String | 最优卖价
last | String | 最新成交价
settle | String | 结算价
preSettle | String | 前一日结算价
bidQty | String | 最优买价数量
askQty | String | 最优卖价数量
openInterest | String | 持仓量
volume | String | 成交量
name | String | 产品中文名
timestamp | String | 最新成交时间

## 数字货币实时数据

### 主题订阅

地址 /crypto
一旦对应交易对数据产生，Websocket服务器将通过此订阅主题接口推送至客户端

### 数字货币实时数据更新字段列表

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
ask | String | 最优卖价
ask_qty | String | 最优卖价对应的挂单量
base_volume | String | 成交量基于交易币
bid | String | 最优买价
bid_qty | String | 最优买价对应的挂单量
change | String | 价格变动数额
exchange | String | 交易所
high | String | 最高价
id | String | 交易对加上交易所对应的id
last | String | 最新价
low | String | 今日最低价
open | String | 今日开盘价
percent_change | String | 今日涨跌幅
pre_close | String | 昨日收盘价
quote_volume | String | 成交量基于基础币
symbol | String | 交易对代码
timestamp | String | 最新成交时间

## 外汇实时数据

### 主题订阅

地址 /forex
一旦对应交易对数据产生，Websocket服务器将通过此订阅主题接口推送至客户端

### 外汇实时数据更新字段列表

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
ask | String | 最优卖价
bid | String | 最优买价
id | String | 品种所对应的id
symbol | String | 交易对代码
timestamp | String | 最新成交时间


# 经济数据接口

## 简要说明

部分国家的宏观经济数据,包括日,月,季度,年份周期的各类经济指标

经济数据接口base url: api.trochil.cn

## 单个国家的所有指标

> 示例代码：查询单个国家的所有指标

```python
import requests

info = requests.get('http://api.trochil.cn/v1/indicator/country', params={'country': '美国'})
```

> 返回结果：查询单个国家的所有指标

```json
{
    'code': 200,
    'data': {
        'country': '美国',
        'category': {
            '国民收入与支出': [
                {'country': '美国', 'series_code': '548b08363354c0c2d9d0540dcda2173c', 'series_name': '美国实际GDP',
                 'unit': '10亿美元(2012年不变价格)', 'freq': 'Q', 'sa': '季调年化', 'source': '美国经济分析局', 'max_value': 19221.97,
                 'min_value': 2023.452, 'last_value': 18987.877, 'last_time': '2020-01-01 00:00:00'},
                {'country': '美国', 'series_code': 'e4e9d188ae428e0855c359c46419edc1', 'series_name': '美国联邦政府债务：总公共债务',
                 'unit': '百万美元', 'freq': 'Q', 'sa': '非季调', 'source': '美国财政部', 'max_value': 23201380.0,
                 'min_value': 316097.0, 'last_value': 23201380.0, 'last_time': '2019-10-01 00:00:00'}...],
            '就业与劳动力市场': [
                {'country': '美国', 'series_code': '6512b2ecd8f003e5ae0349519e0e8752', 'series_name': '美国非农就业人数',
                 'unit': '千人', 'freq': 'M', 'sa': '季调', 'source': '美国劳工统计局', 'max_value': 152442.0, 'min_value': 38507.0,
                 'last_value': 131072.0, 'last_time': '2020-04-01 00:00:00'},
                {'country': '美国', 'series_code': '66934455e0239fda92e205b25b5d4ad5', 'series_name': '美国非农商业部门单位劳动力成本',
                 'unit': '指数（2012=100）', 'freq': 'Q', 'sa': '季调', 'source': '美国劳工统计局', 'max_value': 112.96,
                 'min_value': 15.965, 'last_value': 112.96, 'last_time': '2020-01-01 00:00:00'}...],
        }
        ...
    }
}
```

获取单个国家的所有指标。

### HTTP请求

`GET /v1/indicator/country`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
country | String | True | NA | 国家名称如美国

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
data | dict | 对应国家的所有经济指标信息
code | int | 请求结果对应的状态码

### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
category | String | 经济指标目录
country | String | 国家名称
series_code | String | 由指标名称经过算法加密而成的唯一标识码
series_name | String | 指标名称
unit | String | 指标衡量计数单位
freq | String | 指标衡量时间单位
sa | String | 指标衡量时间单位中文
source | String | 指标数据来源
max_value | Float | 统计内指标最大值
min_value | Float | 统计内指标最小值
last_value | Float | 指标最新值
last_time | String | 指标最新时间

## 获取单个指标

> 示例代码：查询单个指标

```python
import requests

info = requests.get('http://api.trochil.cn/v1/indicator/single',
                    params={'series_code': '548b08363354c0c2d9d0540dcda2173c'})
```

> 返回结果：查询单个指标

```json
{
    'code': 200,
    'data': {
        'country': '美国',
        'category': '国民收入与支出',
        'series_code': '548b08363354c0c2d9d0540dcda2173c',
        'series_name': '美国实际GDP',
        'unit': '10亿美元(2012年不变价格)',
        'freq': 'Q', 'sa': '季调年化',
        'source': '美国经济分析局',
        'values': [
            {'date': '1947-01-01', 'value': 2033.061},
            {'date': '1947-04-01', 'value': 2027.639},
            {'date': '1947-07-01', 'value': 2023.452},
            {'date': '1947-10-01', 'value': 2055.103},
            ...
            {'date': '2019-10-01', 'value': 19221.97},
            {'date': '2020-01-01', 'value': 18987.877}],
        'transform': 'lin'
    }
}
```

获取某个国家的单个指标。

### HTTP请求

`GET /v1/indicator/single`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
series_code | String | True | NA | 指标名称对应的唯一代码
freq | String | No | 原始频率 | 指标对应的时间频率,freq与公布数据的频率不<br>一致时采用重取样的方式生成,可用频率为D, W, M, Q，A
transform | String | No | lin | 计算方式,lin-原始值,chg-环比增长,ch1-同比增长<br>,pch-环比增长率,pc1-同比增长率,pca-复合年增长率,<br>cch-连续复合增长率,cca-连续复合年增长率,log-自然对数
start_date | String | No | 最早时间 | 开始日期
end_date | String | No | 当前日期 | 结束日期

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
data | dict | 对应国家的所有经济指标信息
code | int | 请求结果对应的状态码

### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
country | String | 国家名称
category | String | 经济指标目录
series_code | String | 由series_name经过算法加密而成的唯一标识码
series_name | String | 指标名称
unit | String | 指标衡量计数单位
freq | String | 指标衡量时间单位
sa | String | 指标衡量时间单位中文
source | String | 指标数据来源
values | list | 请求时间内对应时间的指标数据
transform | String | 指标计算方式


# 全市场快照

## 简要说明

支持获取美股,A股,港股,期货,数字货币,外汇市场内所有交易对的ticker价格快照数据

经济数据接口base url: api.trochil.cn

## 单个市场类型全部交易对价格快照

> 示例代码：查询单个市场类型全部交易对价格快照

```python
import requests

info = requests.get('http://api.trochil.cn/v1/snapshot/market', params={'market': 'usstock'})
```

> 返回结果：查询单个市场类型全部交易对价格快照

```json
{
    'code': 200,
    'data': [
        {'name': 'SIFCO Industries, Inc.', 'symbol': 'SIF', 'last': 4.14, 'change': 1.94, 'percent_change': 87.97,
         'pre_close': 2.2, 'open': 3.54, 'high': 5.5, 'low': 3.54, 'volume': 17573373.0, 'mktcap': 24255415.0,
         'pe': -3.0218977, 'exchange': 'NYSE'},
        {'name': 'Genius Brands International, Inc.', 'symbol': 'GNUS', 'last': 1.31, 'change': 0.58,
         'percent_change': 79.85, 'pre_close': 0.73, 'open': 0.77, 'high': 1.41, 'low': 0.71, 'volume': 109526316.0,
         'mktcap': 38765855.0, 'pe': -1.0396825, 'exchange': 'NASDAQ'},
        {'name': 'United Natural Foods, Inc.', 'symbol': 'UNFI', 'last': 22.59, 'change': 7.08, 'percent_change': 45.65,
         'pre_close': 15.51, 'open': 17.06, 'high': 23.38, 'low': 17.06, 'volume': 23196133.0, 'mktcap': 1211228008.0,
         'pe': -3.58003175, 'exchange': 'NASDAQ'},
        {'name': 'RumbleOn, Inc.', 'symbol': 'RMBL', 'last': 0.58, 'change': 0.17, 'percent_change': 42.09,
         'pre_close': 0.41, 'open': 0.42, 'high': 0.61, 'low': 0.4, 'volume': 31087517.0, 'mktcap': 14173489.0,
         'pe': -0.28571428, 'exchange': 'NASDAQ'}
        ...
    ]
}
```

获取单个市场类型全部交易对价格快照信息。

### HTTP请求

`GET /v1/snapshot/market`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
market | String | True | NA | 市场类型目前可选(usstock,hkstock,cnstock,crypto,futures,forex)

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
data | dict | 对应国家的所有经济指标信息
code | int | 请求结果对应的状态码

### data说明

市场类型不同data中包含的字段有所差异,如没有某字段则说明该市场类型数据没有该字段
如期货市场没有pe,mktcap等字段

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
name | String | 交易对名称
symbol | String | 交易对简称
last | String | 最新价
change | String | 价格变动数
percent_change | String | 价格变动比率
pre_close | String | 前一个交易日收盘价
open | String | 开盘价
high | String | 最高价
low | Float | 最低价
volume | Float | 成交量
mktcap | Float | 市值
pe | String | 市盈率
exchange | String | 交易所

## 单个市场类型涨跌幅榜

> 示例代码：查询单个市场类型涨跌幅榜单

```python
import requests

info = requests.get('http://api.trochil.cn/v1/snapshot/top', params={'market': 'usstock', 'lenth': 10})
```

> 查询单个市场类型涨跌幅榜单

```json
{
    'code': 200,
    'data': [
        {'name': 'SIFCO Industries, Inc.', 'symbol': 'SIF', 'last': 4.14, 'change': 1.94, 'percent_change': 87.97,
         'pre_close': 2.2, 'open': 3.54, 'high': 5.5, 'low': 3.54, 'volume': 17573373.0, 'mktcap': 24255415.0,
         'pe': -3.0218977, 'exchange': 'NYSE'},
        {'name': 'Genius Brands International, Inc.', 'symbol': 'GNUS', 'last': 1.31, 'change': 0.58,
         'percent_change': 79.85, 'pre_close': 0.73, 'open': 0.77, 'high': 1.41, 'low': 0.71, 'volume': 109526316.0,
         'mktcap': 38765855.0, 'pe': -1.0396825, 'exchange': 'NASDAQ'},
        {'name': 'United Natural Foods, Inc.', 'symbol': 'UNFI', 'last': 22.59, 'change': 7.08, 'percent_change': 45.65,
         'pre_close': 15.51, 'open': 17.06, 'high': 23.38, 'low': 17.06, 'volume': 23196133.0, 'mktcap': 1211228008.0,
         'pe': -3.58003175, 'exchange': 'NASDAQ'},
        {'name': 'RumbleOn, Inc.', 'symbol': 'RMBL', 'last': 0.58, 'change': 0.17, 'percent_change': 42.09,
         'pre_close': 0.41, 'open': 0.42, 'high': 0.61, 'low': 0.4, 'volume': 31087517.0, 'mktcap': 14173489.0,
         'pe': -0.28571428, 'exchange': 'NASDAQ'}
        ...
    ]
}
```

获取查询单个市场类型涨跌幅榜单信息。

### HTTP请求

`GET /v1/snapshot/top`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
market | String | True | NA | 市场类型目前可选(usstock,hkstock,cnstock,crypto,futures,forex)
top | String | No | gainers | 涨跌幅排行榜,涨-gainers,跌-losers
lenth | int | No | 全部数据 | 榜单前x名

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
data | dict | 对应国家的所有经济指标信息
code | int | 请求结果对应的状态码

### data说明

市场类型不同data中包含的字段有所差异,如没有某字段则说明该市场类型数据没有该字段
如期货市场没有pe,mktcap等字段

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
name | String | 交易对名称
symbol | String | 交易对简称
last | String | 最新价
change | String | 价格变动数
percent_change | String | 价格变动比率
pre_close | String | 前一个交易日收盘价
open | String | 开盘价
high | String | 最高价
low | Float | 最低价
volume | Float | 成交量
mktcap | Float | 市值
pe | String | 市盈率
exchange | String | 交易所


# 股指数据

## 简要说明

支持获取全球股指实时数据以及全球股指历史日线级别数据,全球股指历史数据分钟级别待扩展

经济数据接口base url: api.trochil.cn

## 全球股指实时数据

> 示例代码：查询全球股指实时价格快照

```python
import requests

info = requests.get('http://api.trochil.cn/v1/index/tickers')
```

> 返回结果：查询全球股指实时价格快照

```json
{
    'code': 200,
    'data': [
        {'country': '中国', 'name': '上证指数', 'last': 2891.36, 'high': 2896.47, 'low': 2883.6, 'change': -7.22,
         'percent_change': -0.25, 'time': '2020-05-20 13:40:53'},
        {'country': '中国', 'name': '深证成指', 'last': 10992.75, 'high': 11056.98, 'low': 10970.22, 'change': -60.09,
         'percent_change': -0.54, 'time': '2020-05-20 13:10:48'},
        {'country': '中国', 'name': '富时中国A50指数', 'last': 13491.66, 'high': 13499.27, 'low': 13417.79, 'change': 9.75,
         'percent_change': 0.07, 'time': '2020-05-20 13:40:00'},
        {'country': '越南', 'name': '越南HNX30', 'last': 212.41, 'high': 213.64, 'low': 208.73, 'change': 0.0,
         'percent_change': 0.0, 'time': '2020-05-18 13:00:00'},
        ...
    ]
}
```

获取全球股指实时价格快照信息。

### HTTP请求

`GET /v1/index/tickers`

### 请求参数

无

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
data | dict | 对应国家的所有经济指标信息
code | int | 请求结果对应的状态码

### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
country | String | 股指所属国家名称
name | String | 股指名称
last | Float | 最新价
high | Float | 最高价
low | Float | 最低价
change | Float | 价格变动数
percent_change | Float | 价格变动比例
time | String | 时间

## 全球股指历史数据

> 示例代码：查询全球股指历史数据

```python
import requests

info = requests.get('http://api.trochil.cn/v1/index/daily',
                    params={'symbol': 'nas100', 'start_date': '2020-01-01', 'end_date': '2020-02-01'})
```

> 返回结果：查询全球股指历史数据

```json
{
    'code': 200,
    'data': [
        {'datetime': '2020-01-02', 'open': 8802.22, 'high': 8873.63, 'low': 8786.9, 'close': 8872.22,
         'volume': 152650000.0},
        {'datetime': '2020-01-03', 'open': 8755.17, 'high': 8843.65, 'low': 8755.17, 'close': 8793.9,
         'volume': 144750000.0},
        ...
        {'datetime': '2020-01-31', 'open': 9169.91, 'high': 9170.22, 'low': 8961.59, 'close': 8991.51,
         'volume': 217220000.0}
    ]
}
```

获取全球股指历史数据。

### HTTP请求

`GET /v1/index/daily`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 股指代码,如nas100
start_date | String | False | 数据起始时间 | 样本起始时间,如2019-01-01
end_date | String | False | 最近更新时间 | 样本结束时间,如2019-09-09


### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
data | dict | 对应国家的所有经济指标信息
code | int | 请求结果对应的状态码

### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
datetime | String | k线时间
open | Float | 开盘价
high | Float | 最高价
low | Float | 最低价
close | Float | 收盘价
volume | Float | 交易量


# 快讯新闻流

## 简要说明

覆盖A股,港股,美股,期货,数字货币,外汇,宏观经济实时新闻,包括A股,港股和美股的大部分个股新闻
各市场类型对应的代码为:    

A股: cnstock, 港股: hkstock,  期货: cnfuture, 美股: usstock, 数字货币: crypto, 外汇: forex, 宏观经济: economics

经济数据接口base url: api.trochil.cn

## 实时新闻讯息

> 示例代码：查询实时新闻讯息

```python
import requests

info = requests.get('http://api.trochil.cn/v1/news')
```

> 返回结果：查询实时新闻讯息

```json
{
    'code': 200,
    'data':
        [
            {'title': 'XXX',
             'authors': None,
             'published': '2020-06-10 06:30:50',
             'content': 'XXX',
             'content_html': 'XXX', 'category': 'news', 'url': 'XXX', 'source': '东方财富网', 'subcat': 'cnstock',
             'individual': 'SH600000'
             }
        ]
}
```

获取各市场类型新闻快讯讯息。

### HTTP请求

`GET /v1/news`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
limit | int | False | 10 | 获取新闻条数,最大为10条
subcat | String | False | 全市场类型 | 市场类型如cnstock,cnfuture,crypto等
category | String | False | NA | 快讯或者深度文章,news为快讯,article为深度文章
individual | String | False | NA | 个股代码,目前仅支持A股,港股,美股,如SH600000

### 响应数据

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
data | list | 对应新闻的描述
code | int | 请求结果对应的状态码

### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
title | String | 新闻标题
authors | String | 新闻作者
published | Float | 新闻发布时间
content | Float | 纯文本新闻
content_html | Float | 带标签新闻
individual | Float | 个股标签
