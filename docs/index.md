# Virgox Api

* 本篇列出REST接口的baseurl:
    **https://www.virgox.net**
* 所有接口的响应都是JSON格式
* 为保证接口正常访问，需联系客服提供ip地址


----
## 接口鉴权和签名 

对于私有接口除了接口本身所需的参数外，需要传的参数包括`apiKey`，和`sign`，除去`sign`之外的参数都需要参与签名。

| 参数        | 类型   |  是否必须   |  说明   |
| :--------:   | :-----:  |  :-----:  |  :-----:  |
| apiKey         | string   |  是   |  您的apiKey   |
| sign          | string   |  是   |  请求签名   |

### 签名规则 
* 1.将除sign以外的参数按参数名进行字典排序。
```json
{
    "apiKey":"AAAAA",
    "apiSecret":"BBBBB",
    "category":"1",
    "price":"9000.1",
    "qty":"1.2",
    "symbol":"BTC/CAD",
    "type":"1"
}
```
* 排序后按顺序拼接参数的值AAAAABBBBB19000.11.2BTC/CAD1。

* 对得到的值进行md5加密得到：b60743ad70bad7d1fc47777c0d58604e。
 
* 将md5后得到的值加入sign中作为参数传递。


## **K线数据(蜡烛图)**

* GET `/apis/api/market/history/kline`

**请求参数**

| 参数        | 类型   |  是否必须   |  说明   |
| :--------:   | :-----:  |  :-----:  |  :-----:  |
| symbol  | String  | 是  | 市场交易对,以”/”分开(如ETH/BTC)  |
| period  | Int  | 是  | K线类型(1分钟 1 ，5分钟 5 ，15分钟 15 ，30分钟 30 ，1小时 60 ， 1天 1440 ，1周 10080，1个月 43200)  |

**返回值**

```json
{
    "code":0,
    "msg":"success",
    "success":true,
    "data":[
        {
            "id":1,
            "marketId":21,
            "open":1,
            "high":1,
            "low":1,
            "close":1,
            "qty":1,
            "createTime":1557211356000
        }
    ]
}
```

**返回值说明**

| 返回值        |  类型   | 说明 |
| :--------:   | :-----:  | :-----:  |
| id        |  Int   | K线id   |
| marketId        |  Int   | 市场id   |
| low        |  Decimal   | 市场最低价   |
| high        |  Decimal   | 市场最高价   |
| close        |  Decimal   | 市场收盘价   |
| open       |  Decimal   | 市场开盘价   |
| qty       |  Decimal   | 交易数量   |
| createTime       |  Long   | 时间戳   |


## **聚合行情(Ticker)**

* GET `/apis/api/market/detail/merged`

**请求参数**

| 参数        | 类型   |  是否必须   |  说明   |
| :--------:   | :-----:  |  :-----:  |  :-----:  |
| symbol  | String  |  是 | 市场名称 币种名称后面需要加"/"和板块名称(btc/usdt)  |

**返回值**

```json
{
    "code":0,
    "msg":"success",
    "data":{
        "volume":85.98,
        "symbol":"BTC/USDT",
        "high":8889.87,
        "last":8794.39,
        "low":8709.73,
        "buy":8710.99,
        "sell":8840.69,
        "id":91,
        "changeRate":"+0.08%",
        "open":8786.49
    },
    "success":true
}
```

**返回值说明**

| 返回值        |  类型   | 说明 |
| :--------:   | :-----:  | :-----:  |
| id        |  Int   | 市场id   |
| low        |  Decimal   | 市场最低价   |
| high        |  Decimal   | 市场最高价   |
| last        |  Decimal   | 市场最新价   |
| open       |  Decimal   | 市场开盘价   |
| sell       |  Decimal   | 卖一价   |
| buy       |  Long   | 买一价   |
| volume       |  Decimal   | 成交额（计价货币量） |
| symbol       |  String   | 交易对   |
| changeRate       |  Decimal   | 涨跌幅   |


## **所有交易对的最新Tickers**

* GET `/apis/api/market/tickers`

**请求参数**

| 参数        | 类型   |  是否必须   |  说明   |
| :--------:   | :-----:  |  :-----:  |  :-----:  |
| none        | none   |  none   |  none   |

**返回值**

```json
{
    "code":0,
    "msg":"success",
    "data":[
        {
            "volume":83.1,
            "symbol":"BTC/USDT",
            "high":8889.87,
            "last":8787.47,
            "low":8709.73,
            "buy":8714.19,
            "sell":8848.52,
            "id":91,
            "changeRate":"+0.01%",
            "open":8786.49
        },
        {
            "volume":0,
            "symbol":"ETH/USDT",
            "high":0,
            "last":213.82,
            "low":0,
            "buy":212.09,
            "sell":214.9,
            "id":90,
            "changeRate":"+0.00%",
            "open":0
        }
    ],
    "success":true
}
```

**返回值说明**

| 返回值        |  类型   | 说明 |
| :--------:   | :-----:  | :-----:  |
| id        |  Int   | 市场id   |
| low        |  Decimal   | 市场最低价   |
| high        |  Decimal   | 市场最高价   |
| last        |  Decimal   | 市场最新价   |
| open       |  Decimal   | 市场开盘价   |
| volume       |  Decimal   | 成交额（计价货币量） |
| symbol       |  String   | 交易对   |
| changeRate       |  Decimal   | 涨跌幅   |
| sell       |  Decimal   | 卖一价   |
| buy       |  Long   | 买一价   |


## **市场深度**

* GET `/apis/api/market/depth`

**请求参数**

| 参数        | 类型   |  是否必须   |  说明   |
| :--------:   | :-----:  |  :-----:  |  :-----:  |
| symbol        | String   |  是   |  市场名称 币种名称后面需要加"/"和板块名称(btc/usdt)  |

**返回值**

```json
{
    "code":0,
    "msg":"success",
    "data":{
        "asks":[
            {
                "id":"3229af464d014b588e4d12b288d96c9e",
                "marketId":8764.78,
                "price":8764.78,
                "qty":0.084783,
                "volume":743.10434274
            },
            {
                "id":"6f93f30c1289441aa5756affe33c849b",
                "marketId":8764.78,
                "price":8765.88,
                "qty":0.050587,
                "volume":443.43957156
            }
        ],
        "ts":1590401000781
    },
    "success":true
}
```

**返回值说明**

| 返回值        |  类型   | 说明 |
| :--------:   | :-----:  | :-----:  |
| id        |  String   | id   |
| marketId        |  Int   | 市场id   |
| price       |  Decimal   | 价格   |
| volume       |  Decimal   | 销售额   |
| qty       |  Decimal   | 数量   |


## **市场最近成交记录**

* GET `/apis/api/market/trade`

**请求参数**

| 参数        | 类型   |  是否必须   |  说明   |
| :--------:   | :-----:  |  :-----:  |  :-----:  |
| symbol        | String   |  是  |  市场名称 币种名称后面需要加"/"和板块名称(btc/usdt)   |

**返回值**

```json
{
    "code":0,
    "msg":"success",
    "data":{
        "tradeList":[
            {
                "id":794780,
                "qty":0.060999,
                "price":8868.13,
                "type":2,
                "createTime":1590399172000,
                "doublePrice":8868.13
            },
            {
                "id":794776,
                "qty":0.022285,
                "price":8870.43,
                "type":2,
                "createTime":1590399158000,
                "doublePrice":8870.43
            }
        ],
        "ts":1590399185283
    },
    "success":true
}
```

**返回值说明**

| 返回值        |  类型   | 说明 |
| :--------:   | :-----:  | :-----:  |
| id        |  String   | id   |
| price       |  Decimal   | 价格   |
| qty       |  Decimal   | 数量   |
| type       |  Int   | 买卖类型 1 买 2 卖   |
| createTime       |  Long   | 成交时间戳   |
| doublePrice       |  double   | 成交价格 double类型  |


## **下单**

* POST `/apis/api/member/addOrder`
***(注意：API下单时需关闭交易密码)***

**请求参数**

| 参数        | 类型   |  是否必须   |  说明   |
| :--------:   | :-----:  |  :-----:  |  :-----:  |
| apiKey        | String   |  是  |  用户申请的密钥 |
| sign        | String   |  是  |  参数签名|
| symbol        | String   |  是  | 交易对(如btc/usdt) |
| category        | Int   |  是  |1 限价单 2市价单|
| type        | Int   |  是  |1 买入 2 卖出|
| price        | Decimal   |  否  |价格(当订单类型是限价单时需传入，即 category=1时)   |
| total        | Decimal   |  否  |  总额(当订单类型是市价单时并且是买入时传入，即 category=2并且type=1时total需要传入  |
| qty        | Decimal   |  否  |  数量(限价单的买入卖出，市价单的卖出时传入)   |


**返回值说明**

```json
{
    "code":0,
    "msg":"success",
    "data":{
        "orderId":15
    },
    "success":true
}
```

| 返回值        |  类型   | 说明 |
| :--------:   | :-----:  | :-----:  |
| orderId        |  String   | 订单id   |


## **撤销订单**

* GET `/apis/api/member/cancelOrder`

**请求参数**

| 参数        | 类型   |  是否必须   |  说明   |
| :--------:   | :-----:  |  :-----:  |  :-----:  |
| apiKey        | String   |  是  |  用户申请的密钥 |
| sign        | String   |  是  |  参数签名|
| id        | String   |  是  | 订单id |


**返回值**

```json
{
    "code":0,
    "msg":"success",
    "data":"OrderCancelled!",
    "success":true
}
```

**返回值说明**

| 返回值        |  类型   | 说明 |
| :--------:   | :-----:  | :-----:  |
| none        |  none   | none   |


## **账户信息**

* GET `/apis/api/member/accounts`

**请求参数**

| 参数        | 类型   |  是否必须   |  说明   |
| :--------:   | :-----:  |  :-----:  |  :-----:  |
| apiKey        | String   |  是  |  用户申请的密钥 |
| sign        | String   |  是  |  参数签名|

**返回值**

```json
{
    "code":0,
    "msg":"success",
    "data":[
        {
            "total":0,
            "balance":0,
            "coinName":"ETH",
            "freezingBalance":0
        },
        {
            "total":0,
            "balance":0,
            "coinName":"BTC",
            "freezingBalance":0
        },
        {
            "total":10000,
            "balance":10000,
            "coinName":"USDT",
            "freezingBalance":0
        },
        {
            "total":0,
            "balance":0,
            "coinName":"PAX",
            "freezingBalance":0
        },
        {
            "total":0,
            "balance":0,
            "coinName":"USDC",
            "freezingBalance":0
        }
    ],
    "success":true
}
```

**返回值说明**

| 返回值        |  类型   | 说明 |
| :--------:   | :-----:  | :-----:  |
| balance        |  Decimal   | 可用余额   |
| freezingBalance        |  Decimal   | 冻结余额   |
| total        |  Decimal   | 总余额   |
| coinName        |  String   | 币种名称   |

## **查询用户订单**

* GET `/apis/api/member/queryOrder`

**请求参数**

| 参数        | 类型   |  是否必须   |  说明   |
| :--------:   | :-----:  |  :-----:  |  :-----:  |
| apiKey        | String   |  是  |  用户申请的密钥 |
| sign        | String   |  是  |  参数签名|
| symbol        | String   |  是  |  交易对|
| status        | Int   |  是  |  订单状态-1已撤单 0已下单 1待撮合 2撮合中 3已完成撮合|

**返回值**

```json
{
    "code":0,
    "msg":"success",
    "data":[
        {
            "createTime":1593602042000,
            "price":9224.18,
            "qty":0.079137,
            "tradeQty":0,
            "id":44788514,
            "type":1,
            "direction":2,
            "status":0
        },
        {
            "createTime":1593602042000,
            "price":9105.72,
            "qty":0.083102,
            "tradeQty":0,
            "id":44788513,
            "type":1,
            "direction":1,
            "status":1
        }
    ],
    "success":true
}
```

**返回值说明**

| 返回值        |  类型   | 说明 |
| :--------:   | :-----:  | :-----:  |
| id        |  Int   | 订单id   |
| type        |  Int   | 订单类型  1 限价 2 市价   |
| direction        |  Int   | 方向 1买 2 卖   |
| price        |  Decimal   | 下单价格   |
| qty        |  Decimal   | 下单数量   |
| tradeQty        |  Decimal   | 已成交数量   |
| status        |  Int   | 状态-1已撤单 0已下单 1待撮合 2撮合中 3已完成撮合   |
| createTime        |  Long   | 订单生成时间戳   |


## **查询用户成交记录**

* GET `/apis/api/member/queryTrade`

**请求参数**

| 参数        | 类型   |  是否必须   |  说明   |
| :--------:   | :-----:  |  :-----:  |  :-----:  |
| apiKey        | String   |  是  |  用户申请的密钥 |
| sign        | String   |  是  |  参数签名|
| symbol        | String   |  是  |  交易对|


**返回值**

```json
{
    "code":0,
    "msg":"success",
    "data":[
        {
            "id":1,
            "amount":12,
            "qty":1,
            "price":12,
            "type":"buy",
            "createTime":"2021-02-22 11:21:07",
            "createTimeMs":1613964067000
        }
    ],
    "success":true
}
```

**返回值说明**

| 返回值        |  类型   | 说明 |
| :--------:   | :-----:  | :-----:  |
| id        |  Int   | id   |
| qty        |  Decimal   | 成交数量  |
| amount        |  Decimal   | 销售额  |
| type        |  String   | 买卖类型 Buy sell  |
| price        |  Decimal   | 成交价格   |
| createTimeMs        |  Long   | 成交时间 毫秒   |
| createTime        |  String   | 成交时间   |


## **查询市场详情**

* GET `/apis/api/marketInfo`

**请求参数**

| 参数        | 类型   |  是否必须   |  说明   |
| :--------:   | :-----:  |  :-----:  |  :-----:  |
| symbol        | String   |  否  |  交易对|

**返回值**

```json
{
    "code":0,
    "msg":"success",
    "data":[
        {
            "id":91,
            "marketName":"BTC/USDT",
            "categoryCoin":"USDT",
            "marketCoin":"BTC",
            "minTotal":1,
            "maxTotal":10000,
            "minQty":0.0001,
            "maxQty":0.31,
            "priceDecimals":2,
            "qtyDecimals":6
        }
    ],
    "success":true
}
```

**返回值说明**

| 返回值        |  类型   | 说明 |
| :--------:   | :-----:  | :-----:  |
| id        |  Int   | id   |
| marketName        |  String   | 市场名称  |
| categoryCoin        |  String   | 计价货币  |
| marketCoin        |  String   | 基础货币  |
| minTotal        |  Decimal   | 最小交易额  |
| maxTotal        |  Decimal   | 最大交易额  |
| minQty        |  Decimal   | 最小交易数   |
| maxQty        |  Decimal   | 最大交易数   |
| priceDecimal        |  Decimal   | 价格小数位   |
| qtyDecimal        |  Decimal   | 数量小数位   |

## **查询订单详情**

* GET `/apis/api/member/queryOrderInfo`

**请求参数**

| 参数        | 类型   |  是否必须   |  说明   |
| :--------:   | :-----:  |  :-----:  |  :-----:  |
| apikey        | String   |  是  |  apiKey|
| sign        | String   |  是  |  参数签名|
| orderId        | String   |  是  |  订单id|

**返回值**

```json
{
    "code":0,
    "msg":"success",
    "data":{
        "id":"20405197",
        "direction":1,
        "type":1,
        "price":0.972893,
        "qty":273.4,
        "tradeQty":0,
        "status":1,
        "detail":[
            {
                "amount":663.3669056,
                "create_time":1599883383000,
                "price":10351.36,
                "qty":0.064085,
                "fee":0,
                "feeCoinName":"USDT",
                "id":2,
                "createTimeMs":1599883383000
            }
        ],
        "createTime":1604687659000
    },
    "success":true
}
```

**返回值说明**

| 返回值        |  类型   | 说明 |
| :--------:   | :-----:  | :-----:  |
| id        |  Int   | 订单id   |
| direction        |  Int   | 订单方向  1 买入 2 卖出 |
| type        |  Int   | 订单分类 1 限价 2 市价 |
| price        |  Decimal   | 下单价格  |
| qty        |  Decimal   | 下单数   |
| tradeQty        |  Decimal   | 已成交数   |
| status        |  Int   | 状态   |
| detail        |  Object   |  成交详情  |
| amount        |  Decimal   | 成交额   |
| createTime        |  Long   | 成交时间   |
| price         |  Decimal   | 成交价格   |
| qty        |  Decimal   | 成交数量   |
| fee        |  Decimal   | 手续费   |
| feeCoinName        |  String   | 手续费币种   |
| id        |  String   | 成交id   |
| createTimeMs        |  Long   | 成交时间 毫秒   |


## **查询充值记录**

* POST `/apis/api/member/rechargeRecords`

**请求参数**

| 参数        | 类型   |  是否必须   |  说明   |
| :--------:   | :-----:  |  :-----:  |  :-----:  |
| apikey        | String   |  是  |  apiKey|
| sign        | String   |  是  |  参数签名|
| coinName        | String   |  否  |  币种名称|

**返回值**

```json
{
    "code": 0,
    "msg": "success",
    "data": {
        "total": 3,
        "list": [
            {
                "coinType": 4,
                "blockConfirm": 0,
                "memo": null,
                "txId": null,
                "remark": null,
                "updateTime": 1612775730,
                "type": 1,
                "toAddress": null,
                "coinId": 10,
                "createTime": 1612775702,
                "browser": "https://www.omniexplorer.info/tx/",
                "qty": 100000.000000000000000,
                "fromAddress": null,
                "id": 3,
                "coinName": "USDT",
                "blockConfirmTotal": 2,
                "status": "completed"
            },
            {
                "coinType": 2,
                "blockConfirm": 0,
                "memo": null,
                "txId": null,
                "remark": null,
                "updateTime": 1612775730,
                "type": 1,
                "toAddress": null,
                "coinId": 2,
                "createTime": 1612775702,
                "browser": "https://btc.com/",
                "qty": 100.000000000000000,
                "fromAddress": null,
                "id": 2,
                "coinName": "BTC",
                "blockConfirmTotal": 2,
                "status": "completed"
            }
        ],
        "pageNum": 1,
        "pageSize": 10,
        "size": 3,
        "startRow": 1,
        "endRow": 3,
        "pages": 1,
        "prePage": 0,
        "nextPage": 0,
        "isFirstPage": true,
        "isLastPage": true,
        "hasPreviousPage": false,
        "hasNextPage": false,
        "navigatePages": 8,
        "navigatepageNums": [
            1
        ],
        "navigateFirstPage": 1,
        "navigateLastPage": 1
    },
    "success": true
}
```

**返回值说明**

| 返回值        |  类型   | 说明 |
| :--------:   | :-----:  | :-----:  |
| id        |  Int   | 充值记录id   |
| coinName        |  String   | 币种名称   |
| status        |  Int   | 充值状态  processing 进行中  completed 已完成  failed 充值失败|
| blockConfirm        |  Int   | 区块确认数 |
| blockConfirmTotal        |  Int   | 入账所需区块确认数 |
| memo        |  Stirng   | memo  |
| txId        |  String   | 哈希值   |
| fromAddress        |  String   | 充值来源地址   |
| toAddress        |  String   | 充值地址   |
| qty        |  Decimal   | 充值数量   |
| browser        |  String   | 区块浏览器地址   |
| createTime        |  Long   | 创建时间   |


## **查询提现记录**

* POST `/apis/api/member/withdrawRecords`

**请求参数**

| 参数        | 类型   |  是否必须   |  说明   |
| :--------:   | :-----:  |  :-----:  |  :-----:  |
| apikey        | String   |  是  |  apiKey|
| sign        | String   |  是  |  参数签名|
| coinName        | String   |  否  |  币种名称|

**返回值**

```json
{
    "code": 0,
    "msg": "success",
    "data": {
        "total": 1,
        "list": [
            {
                "coinType": 3,
                "fee": 5.000000000000000000,
                "memo": null,
                "txId": null,
                "updateTime": 1616990583,
                "toAddress": "0x51e7122ccf2d1dad8328720276858e3525392877",
                "coinId": 20,
                "createTime": 1612878285,
                "browser": "https://eth.btc.com/txinfo/",
                "qty": 2900.000000000000000000,
                "fromAddress": null,
                "id": 2,
                "coinName": "USDT",
                "status": "processing"
            }
        ],
        "pageNum": 1,
        "pageSize": 10,
        "size": 1,
        "startRow": 1,
        "endRow": 1,
        "pages": 1,
        "prePage": 0,
        "nextPage": 0,
        "isFirstPage": true,
        "isLastPage": true,
        "hasPreviousPage": false,
        "hasNextPage": false,
        "navigatePages": 8,
        "navigatepageNums": [
            1
        ],
        "navigateFirstPage": 1,
        "navigateLastPage": 1
    },
    "success": true
}
```

**返回值说明**

| 返回值        |  类型   | 说明 |
| :--------:   | :-----:  | :-----:  |
| id        |  Int   | 提现记录id   |
| coinName        |  String   | 币种名称   |
| status        |  Int   | 提现状态 processing 进行中  completed 已完成  failed 提现失败 |
| memo        |  Stirng   | memo  |
| txId        |  String   | 哈希值   |
| toAddress        |  String   | 提现地址   |
| qty        |  Decimal   | 提现数量   |
| fee        |  Decimal   | 提现手续费 |
| browser        |  String   | 区块浏览器地址   |
| createTime        |  Long   | 提现时间   |
