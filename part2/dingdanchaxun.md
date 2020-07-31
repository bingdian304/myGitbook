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
|out_order_id	|String	|否|	外部订单id，确保唯一性，限定20位以内（查询订单详情）|
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
|pay_date	|String	|是		|订单时间|
|pay_price	|String	|是		|加油金额|
|org_id	|String	|是		|加油站唯一标示|
|org_name	|String	|是		|加油站名称|
|oil_num	|String	|是		|油号|
|oil_gun	|String	|是		|油枪号|

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
            "pay_date":"2019.03.05 10:35:20",
            "pay_price":"300.00",
            "org_id":"972490230234",
            "org_name":"微油科技测试站",
            "org_num":"92#",
            "oil_gun ":"6号油枪"
       },
       …
   ]
}
```


*****
[^Copyright © 微油科技(北京)有限公司 2020 all right reserved，powered by Gitbook]