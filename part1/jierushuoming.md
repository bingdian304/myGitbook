## 油站的准备工作

1）油站提供所在地信息，如加油站名称、地址等基本信息。

2）油站系统按照自身相应规则生成外部订单号，确保订单号的唯一性，现限定10到50位大小写字母和数字的组合，以便做为微油API接口的外部订单号。

3）油站系统提供回调接口，以便接收微油平台的支付结果通知。


## 微油的准备工作

1）微油提供加油业务相关的API接口，以供油站系统调用。

2）微油将订单状态异步回调给油站系统的回调接口。

3）微油提供仅限油站系统使用的系统参数，如油站唯一标示、RSA加密公钥等。

4）微油提供测试包，以便油站联调测试。


<!-- *****
[^Copyright © 微油科技(北京)有限公司 2020 all right reserved，powered by Gitbook] -->