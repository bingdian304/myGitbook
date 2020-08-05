## 加油下单

#### 接口地址
http://i.qmanager.cn/gas_interface.php

#### 协议类型
HTTP POST

#### 功能说明
油站扫描付款码后，将加油信息打包调用此接口，传输下单数据，生成一笔加油订单。

#### 接口参数
| 名称 |类型 | 是否必须 | 描述 |
| :--- | :--- | :--- | :--- |
|class |String	|是|	API接口名称GasOilOrderClass|
|opt_type|	String|	是|	固定值 oil_order|
|org_id	|String|	是	|加油站唯一标示（微油提供）|
|total_price	|String	|是|	加油金额（单位：元，大于0的整数或小数，小数须保留小数点后两位）|
|oil_num	|String	|是	|油号，如92#|
|oil_gun	|String	|否	|油枪号，如9，限数字|
|qr_id	|String	|是	|付款码标识，扫码后识别出的参数|
|out_order_id	|String	|是	|外部订单id，确保唯一性，现限定10到50位大小写字母和数字的组合|
|notify_url	|String	|是	|油站的回调地址，记录订单支付状态|

#### 返回结果
|名称	|类型	|是否必须	|描述|
| :--- | :--- | :--- | :--- |
|error_code	|Int	|是		|错误标示码，0成功，其它失败|
|error_msg	|String	|是		|错误信息|
|data	|Array	|是		|数据(订单详细信息)|

#### 返回示例
JSON数据格式
```
{
	"error_code":"0",
	"error_msg":"订单下单成功",
    "out_order_id":"YZ543534534454",                //外部订单id
    "notify_url":"http://youzhan/ordernotify.php",  //油站提供的回调接口
    "data ":{
        "order_id":"149724902302344",        //微油加油订单id
        "create_date":"2020.07.25 10:35:20", //加油订单下单时间
        "total_price":"300.00",             //加油金额
        "org_id":"YZ45511215424512",        //加油站唯一标示
        "org_name":"微油科技测试站",          //加油站名称
   }
}
```


<!-- *****
[^Copyright © 微油科技(北京)有限公司 2020 all right reserved，powered by Gitbook] -->