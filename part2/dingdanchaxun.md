## 加油订单查询

#### 接口地址
http://i.qmanager.cn/gas_interface.php

#### 协议类型
HTTP POST

#### 功能说明
根据油站唯一标示获取订单列表信息。

#### 接口参数
| 名称 |类型 | 是否必须 | 描述 |
| :--- | :--- | :--- | :--- |
|class |String	|是|	API接口名称GasOilOrderClass|
|opt_type|	String|	是|	固定值 query_oil_order|
|org_id	|String|	是	|加油站唯一标示|
|out_order_id	|String	|否|外部订单id，确保唯一性，现限定10到50位大小写字母和数字的组合（用于查询单个订单详情）|
|date	|String	|否	|查询日期，格式yyyy-MM-dd（查询某一日的订单列表）|


#### 返回结果
|名称	|类型	|是否必须	|描述|
| :--- | :--- | :--- | :--- |
|error_code	|Int	|是		|错误标示码，0成功，其它失败|
|error_msg	|String	|是		|错误信息|
|data	|Array	|是		|数据(订单列表信息)|

###### 单笔订单信息结构
|名称	|类型	|是否必须	|描述|
| :--- | :--- | :--- | :--- |
|out_order_id	|String	|是		|外部订单id|
|order_id	|String	|是		|微油订单id|
|org_id	|String	|是		|加油站唯一标示|
|oil_num	|String	|是		|油号|
|oil_gun	|String	|是		|油枪号|
|total_price	|String	|是		|加油金额|
|order_status	|String	|是		|订单状态 1：支付成功 0：待支付 2：支付失败 3：申请退款4：退款成功|
|pay_date	|String	|是		|订单支付时间|
|create_date	|String	|是		|下单时间|

#### 返回示例
JSON数据格式
```
{
	"error_code":"0",
	"error_msg":"订单查询成功",
   "data ":
   [
        {
            "out_order_id":"PA23424234232320234",  
            "order_id":"149724902302344",
            "create_date":"2020-07-28 10:33:29",
            "pay_date":"2020-07-28 10:35:20",
            "total_price":"300.00",
            "org_id":"972490230234",
            "oil_num":"92#",
            "oil_gun ":"6",
            "order_status":"1",
       },
       …
   ]
}
```

<!-- 
*****
[^Copyright © 微油科技(北京)有限公司 2020 all right reserved，powered by Gitbook] -->