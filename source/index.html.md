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

**数字货币和外汇接口会在4月份开放**

**http请求base url:**

api.trochil.com

# API

所有接口均要求认证，注册成为我们的用户后可以在个人主页找到API key，请求数据时传递您的API key作为参数，详情看代码实例。

# A股

## 产品列表

> 示例代码：查询A股产品列表


```python
import requests

info = requests.get('http://api.trochil.com/v1/cnstock/markets',
                    params={'apikey': 'your apikey'})
```


> 返回结果：查询A股产品列表

```json
{
    'timestamp': 1583475638869,
    'data': {
        'SH600000': {'code': 'SH600000', 'name': '浦发银行', 'exchange': '上海证券交易所'},
        'SH600004': {'code': 'SH600004', 'name': '白云机场', 'exchange': '上海证券交易所'},
        ...
    },
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
data | dict | 对应A股的名称和隶属交易所信息
status | String | 请求结果状态

## 实时报价

> 示例代码：查询A股实时报价

```python
import requests

info = requests.get('http://api.trochil.com/v1/cnstock/quote',
                    params={'symbol': 'SZ300817,SZ300820',
                            'apikey': 'your apikey'})
```

> 返回结果：查询A股实时报价

```json
{
    'timestamp': 1583476018603,
    'data': [
        '{"symbol": "SZ300817", "name": "双飞股份", "last": "38.44", "bid": "38.44", "bidQty": "69.0", "ask": "38.61", "askQty": "6.0", "timestamp": "1583476011000"}',
        '{"symbol": "SZ300820", "name": "英杰电气", "last": "132.56", "bid": "132.56", "bidQty": "9287.0", "ask": "0.0", "askQty": "0.0", "timestamp": "1583475990000"}'],
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
data | list | 对应个股品种的报价信息
status | String | 请求结果状态

### data说明

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

## 日图历史

> 示例代码：查询A股日图历史

```python
import requests

info = requests.get('http://api.trochil.com/v1/cnstock/history',
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

info = requests.get('http://api.trochil.com/v1/cnstock/history_one_day',
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

info = requests.get('http://api.trochil.com/v1/cnstock/intraday',
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

info = requests.get('http://api.trochil.com/v1/hkstock/markets',
                    params={'apikey': 'your apikey'})
```

> 返回示例：查询港股产品列表:

```json
{
    'timestamp': 1583478480228,
    'data': {
        'HK00001': {'code': 'HK00001', 'name': '长和', 'exchange': '香港证券交易所'},
        'HK00002': {'code': 'HK00002', 'name': '中电控股', 'exchange': '香港证券交易所'},
        ...
    },
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
data | dict | 对应港股的名称和隶属交易所信息
status | String | 请求结果状态

## 实时报价

> 示例代码：查询港股实时报价

```python
import requests

info = requests.get('http://api.trochil.com/v1/hkstock/quote',
                    params={
                        'symbol': 'HK09938,HK09968',
                        'apikey': 'your apikey'}
                    )
```

> 示例代码：查询港股实时报价

```json
{
    'timestamp': 1583478603847,
    'data': [
        '{"symbol": "HK09938", "name": "华和控股", "last": "0.85", "timestamp": "1583476040000"}',
        '{"symbol": "HK09968", "name": "汇景控股", "last": "1.94", "timestamp": "1583477955000"}'],
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
data | list | 对应个股品种的报价信息
status | String | 请求结果状态

### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
symbol | String | 股票代码
name | String | 股票中文名称
last | String | 最新成交价
timestamp | String | 最新成交时间

## 日图历史

> 示例代码：查询港股日图历史

```python
import requests

info = requests.get('http://api.trochil.com/v1/hkstock/history',
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

info = requests.get('http://api.trochil.com/v1/hkstock/history_one_day',
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

## 日内数据

> 代码示例：查询港股日内数据

```python
import requests

info = requests.get('http://api.trochil.com/v1/hkstock/intraday',
                    params={
                        'symbol': 'HK09938',
                        'timeframe': '60',
                        'range': 2,
                        'apikey': 'your apikey'}
                    )
```

> 返回结果：查询港股日内数据

```json
{
    'timestamp': 1583479317423,
    'data': [
        {'datetime': '2020-03-04 15:30:00', 'open': 0.8, 'high': 0.8, 'low': 0.8, 'close': 0.8, 'volume': 0.0, 'symbol': 'HK09938'},
        {'datetime': '2020-03-05 09:30:00', 'open': 0.8, 'high': 0.8, 'low': 0.8, 'close': 0.8, 'volume': 85000.0, 'symbol': 'HK09938'},
        {'datetime': '2020-03-05 10:30:00', 'open': 0.78, 'high': 0.78, 'low': 0.77, 'close': 0.77, 'volume': 85000.0, 'symbol': 'HK09938'},
        {'datetime': '2020-03-05 11:30:00', 'open': 0.78, 'high': 0.78, 'low': 0.77, 'close': 0.77, 'volume': 85000.0, 'symbol': 'HK09938'},
        {'datetime': '2020-03-05 12:30:00', 'open': 0.77, 'high': 0.78, 'low': 0.77, 'close': 0.78, 'volume': 10000.0, 'symbol': 'HK09938'},
        {'datetime': '2020-03-05 13:30:00', 'open': 0.77, 'high': 0.78, 'low': 0.77, 'close': 0.78, 'volume': 10000.0, 'symbol': 'HK09938'},
        {'datetime': '2020-03-05 14:30:00', 'open': 0.8, 'high': 0.8, 'low': 0.78, 'close': 0.78, 'volume': 115000.0, 'symbol': 'HK09938'},
        {'datetime': '2020-03-05 15:30:00', 'open': 0.81, 'high': 0.83, 'low': 0.78, 'close': 0.83, 'volume': 425000.0, 'symbol': 'HK09938'}],
    'status': 'ok'
}
```

获取港股的日内K线，同时支持1分钟，5分钟和60分钟。1分钟图最多返回过去7天的数据，5分钟图和60分钟图最多返回过去30天的数据。

### HTTP请求

`GET v1/hkstock/intraday`

### 请求参数

参数名称 | 数据类型 | 是否必须 | 默认值 | 描述
--------- | ------- | ----------- | ----------- | -----------
symbol | String | True | NA | 股票代码,如HK09938
timeframe | int | True | NA | 频率，1分钟为1，5分钟为5，60分钟为60
range | int | True | NA | 查询天数, 7代表7天, 范围1-30,1分钟仅支持7日以内,5分钟60分钟支持30日以内
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
symbol | String | 港股品种代码


# 美股

## 产品列表

> 代码示例：查询美股产品列表

```python
import requests

info = requests.get('http://api.trochil.com/v1/usstock/markets',
                    params={'apikey': 'your apikey'})
```

> 返回结果：查询美股产品列表

```json
{
    'timestamp': 1583480134214,
    'data': {
        'AAPL': {'code': 'AAPL', 'name': '苹果公司', 'exchange': '纳斯达克交易所'},
        'MSFT': {'code': 'MSFT', 'name': '微软公司', 'exchange': '纳斯达克交易所'},
        ...
    },
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
data | dict | 对应美股的名称和隶属交易所信息
status | String | 请求结果状态

## 实时报价

> 示例代码：查询美股实时报价

```python
import requests

info = requests.get('http://api.trochil.com/v1/usstock/quote',
                    params={
                        'symbol': 'BABA,JD',
                        'apikey': 'your apikey'}
                    )
```

> 返回结果：查询美股实时报价

```json
{
    'timestamp': 1583478603847,
    'data': [
        '{"symbol": "BABA", "name": "阿里巴巴", "last": "211.46", "timestamp": "1583476040000"}',
        '{"symbol": "JD", "name": "京东", "last": "44.62", "timestamp": "1583477955000"}'],
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
data | list | 对应个股品种的报价信息
status | String | 请求结果状态

### data说明

字段名称 | 数据类型 | 描述
--------- | ------- | -----------
symbol | String | 股票代码
name | String | 股票中文名称
last | String | 最新成交价
timestamp | String | 实时价格时间

## 日图历史

> 代码示例：查询美股日图历史

```python
import requests

info = requests.get('http://api.trochil.com/v1/usstock/history',
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

info = requests.get('http://api.trochil.com/v1/usstock/history_one_day',
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

info = requests.get('http://api.trochil.com/v1/usstock/intraday',
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
range | int | True | NA | 查询天数, 7代表7天, 范围1-30,1分钟仅支持7日以内,5分钟60分钟支持30日以内
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

info = requests.get('http://api.trochil.com/v1/cnfuture/markets',
                    params={'apikey': 'your apikey'})
```

> 示例代码：查询期货产品列表

```json
{
    'timestamp': 1584587390538,
    'data': {
        'IC2004': {'code': 'IC2004', 'name': '中证500指数2004', 'exchange': '中国金融期货交易所'},
        'IC2006': {'code': 'IC2006', 'name': '中证500指数2006', 'exchange': '中国金融期货交易所'},
        'IC2009': {'code': 'IC2009', 'name': '中证500指数2009', 'exchange': '中国金融期货交易所'},
        'IC0': {'code': 'IC0', 'name': '中证500指数连续', 'exchange': '中国金融期货交易所'},
        'IC2003': {'code': 'IC2003', 'name': '中证500指数2003', 'exchange': '中国金融期货交易所'},
        'IF2004': {'code': 'IF2004', 'name': '沪深300指数2004', 'exchange': '中国金融期货交易所'}},
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
data | dict | 对应合约品种的名称和隶属交易所信息
status | String | 请求结果状态

## 实时报价

> 示例代码：查询期货合约的实时报价

```python
import requests

info = requests.get('http://api.trochil.com/v1/cnfuture/quote',
                    params={
                        'symbol': 'V0,V2002',
                        'apikey': 'your apikey'}
                    )
```

> 示例代码：查询期货合约的实时报价

```json
{
    'timestamp': 1579485232937,
    'data': [
        {
            'symbol_cn': 'PVC连续', 'open': '6480.0', 'high': '6510.0', 'low': '6470.0', 'preClose': '0.0',
            'bid': '6495.0',
            'ask': '6500.0', 'last': '6495.0', 'settle': '0.0', 'preSettle': '6475.0', 'bidQty': '656.0',
            'askQty': '873.0',
            'openInterest': '201404.0', 'volume': '36733.0', 'name': 'PVC', 'datetime': '2020/01/20 095351',
            'symbol': 'V0'
        },
        {
            'symbol_cn': 'PVC2002', 'open': '6435.0', 'high': '6435.0', 'low': '6280.0', 'preClose': '6370.0',
            'bid': '6285.0', 'ask': '6610.0', 'last': '6370.0', 'settle': '0.0', 'preSettle': '6455.0', 'bidQty': '1.0',
            'askQty': '1.0', 'openInterest': '134.0', 'volume': '62.0', 'name': 'PVC', 'datetime': '2020/01/13 150000',
            'symbol': 'V2002'
        }
    ]
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
data | list | 对应合约品种的报价信息
status | String | 请求结果状态

### data说明

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


## 日图历史

> 示例代码：查询期货合约日图历史

```python
import requests

info = requests.get('http://api.trochil.com/v1/cnfuture/history',
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

info = requests.get('http://api.trochil.com/v1/cnfuture/history_one_day',
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

info = requests.get('http://api.trochil.com/v1/cnfuture/intraday',
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

# Websocket行情数据

## 简介

### 接入url

websocket接入地址

ws://stream.trochil.com/ws

### 订阅主题

成功建立与Websocket服务器的连接后，需要进行apikey认证,发送如下消息认证apikey:

{'op': 'auth', 'apikey': your apiKey}

apikey认证通过后Websocket客户端发送如下请求以订阅特定主题,支持以列表形式一次性发送多个主题：

{'op': 'sub', 'topic': topic to sub}

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


# 经济数据接口

## 简要说明

部分国家的宏观经济数据,包括日,月,季度,年份周期的各类经济指标

经济数据接口base url: api.trochil.com

## 单个国家的所有指标

> 示例代码：查询单个国家的所有指标

```python
import requests

info = requests.get('http://api.trochil.com/v1/indicator/country', params={'country': '美国'})
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

info = requests.get('http://api.trochil.com/v1/indicator/single',
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

经济数据接口base url: api.trochil.com

## 单个市场类型全部交易对价格快照

> 示例代码：查询单个市场类型全部交易对价格快照

```python
import requests

info = requests.get('http://api.trochil.com/v1/snapshot/market', params={'market': 'usstock'})
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

info = requests.get('http://api.trochil.com/v1/snapshot/top', params={'market': 'usstock', 'lenth': 10})
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

经济数据接口base url: api.trochil.com

## 全球股指实时数据

> 示例代码：查询全球股指实时价格快照

```python
import requests

info = requests.get('http://api.trochil.com/v1/index/tickers')
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

info = requests.get('http://api.trochil.com/v1/index/daily',
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