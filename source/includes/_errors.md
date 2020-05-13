# Errors

The trochil API uses the following error codes:


Error Code | Meaning
---------- | -------
10002 | 请求日线数据时传递的频率参数错误 -- freq not match daily
10003 | 数据返回的排序参数传递错误 -- sort not in [asc, desc]
10004 | 请求参数中的日线时间格式错误 -- time not format like xxxx-xx-xx
10005 | 请求日内数据时的时间参数格式错误 -- time not format like xxxx-xx-xx xx:xx:xx
10006 | 请求参数中的开始时间大于结束时间 -- start_date is greater than end_date
10007 | 请求需要传递品种代码symbol -- symbol is required
10008 | 请求没有传递时间频率 -- timeframe is required
10009 | 港股和美股请求频率只能是1分钟5分钟和60分钟 -- timeframe should in [1, 5, 60]
10010 | 请求需要传递date参数 -- date is required
10011 | 单次请求多个品种单日历史数据时不能超过数量上限 -- symbol should less than xx
10012 | 请求参数range应该传递正整数 -- range should be type int and > 0
10013 | 非1分钟k线数据请求参数range过大或者过小 -- range should between xx and xx
10014 | 1分钟k线数据请求参数range过大或者过小 -- range should between xx and xx
10015 | 传递的symbol参数不存在 -- symbol is not exist
10016 | 请求没有传递apikey -- apikey is required
10017 | 请求传递的apikey错误 -- apikey is invalid
10018 | 发生了未知异常,请重试或者联系我们 -- An unexpected error has occurred. Please try again
10019 | 免费用户请求1分钟数据时间过长 -- free users can get up to xx days of data before today
10020 | 免费用户请求非1分钟数据时间过长 -- free users can get up to xx days of data before today
10021 | 免费用户没有请求市场类型权限 -- free users have no right to markets
10022 | 今日市场类型请求次数已达到上限 -- The number of markets requests for today has been exhausted
10023 | 今日日线级别数据请求次数已达到上限 -- The number of daily requests for today has been exhausted
10024 | 今日日内级别数据请求次数已达到上限 -- The number of day_inner requests for today has been exhausted
10025 | 免费用户当日总请求次数已达到上限 -- The number of requests for today has been exhausted
10026 | 单次请求实时数据超过限制 -- The number of quote symbols cannot be greater than xx
10027 | A股和期货请求频率只能是5分钟或者60分钟 -- timeframe should in [5, 60]
20010 | 需要传递country参数 -- parameter country is required
20011 | 传递的国家名称有误或者该国家尚无技术指标数据 -- There is no data for this field
20012 | 需要传递series_code参数 -- parameter series_code is required
20013 | 传递的series_code参数不存在或者该指标名称尚无数据 -- There is no data for this field
20014 | 传递的transform参数不存在 -- parameter transform xx is not exist
