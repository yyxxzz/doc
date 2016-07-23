# YohoCoinLogController#
## 一、getYohoCoinNum()方法：获取有货币(mars point)总数 ##
 **请求参数**

| 参数名称 | 参数类型 | 可否为空 |示例  |默认值 |备注  |
| ---------|:--------:| --------:|-----:|------:|-----:|
|uid|int    | 否       |8040028   |-   |用户id|

**实现逻辑：**
1、YohoCoinLogController.getYohoCoinNum()方法中调用users.getYohoCoinNum服务；

2、在YohoCoinCostServiceImpl.getYohoCoin()中进行具体业务逻辑处理，校验uid，若为空，则抛出UID_IS_NULL异常；

3、根据uid查询表yoho_coin_cost，获取该用户的有货币总数(IYohoCoinCostDAO.selectYOHOcoin(uid))；

4、根据uid查询表yoho_coin_cost，获取期限为今年最后一秒的有货币总数(IYohoCoinCostDAO.selectNearExpYOHOcoin(uid, expireTime));

5、构造返回对象YohoCoin，包括有货币总数、即将过期的有货币总数以及有货币的单位.

**示例**

请求：
http://localhost:8080/gateway?method=app.yohocoin.total&uid=8040028&debug=XYZ

返回：

      {
       "alg": "SALT_MD5",
  	   "code": 200,
  	   "data": {
       "code": "20111130-152530",
       "total": 6235400
 	    },
  		"md5": "2dd8d4039a88bbc49ff392ed0e3abc4a",
  		"message": "yoho coin total"
		}

## 二、getYohoCoinLog()方法：获取有货币(mars point)的明细##
 **请求参数**

| 参数名称 | 参数类型 | 可否为空 |示例  |默认值 |备注  |
| ---------|:--------:| --------:|-----:|------:|-----:|
|uid|int    | 否       |8040028   |-   |用户id|
|limit|int    | 是       |-   |20   |每页记录数|
|page|int    | 是       |-   |1   |页数|
|queryType|int    | 是       |-  |0   |查询类型|
|beginTime|string    | 是       |-  |0   |开始时间|
|endTime|string    | 是       |-  |0   |结束时间|

**实现逻辑：**

1、YohoCoinLogController.getYohoCoinLog()方法中调用users.getYohoCoinLog服务；

2、验证uid是否为空，为空则抛出UID_IS_NULL异常；

3、根据limit、page请求参数计算开始记录行数offset；

4、根据请求参数查询库表yoho_coin_history，获取记录总数total和记录信息yohoCoinHistoryList(IYohoCoinHistoryDAO.selectCountByUid,IYohoCoinHistoryDAO.selectByUid)；

5、判断查询的记录信息列表中的Type值若为13（"购买商品赠送：%s"）、14（"晒单奖励：%s"）、15（"补差价：%s"）中的一个，则获取记录信息列表中的params值，根据该值获取商品skn列表；

6、根据skn列表调取product.queryBrowserProduct服务查询获取商品信息，与skn列表结合组装商品信息productMap；

7、判断查询的记录信息列表中的Type值若为19（"兑换实物"）、20（"兑换话费"）、21（"兑换电子券"）中的一个，则获取记录信息列表中的params值，根据该值获取orderCode列表；

8、拼装记录信息返回YohoCoinLogDataRspBO.

**示例**

请求：http://localhost:8080/gateway?method=app.yohocoin.lists&uid=8040028&debug=XYZ

返回：
    
    {
  	 "alg": "SALT_MD5",
     "code": 200,
     "data": {
      "coinlist": [
        {
          "date": "2016-04-27 18:11:12",
          "key": "1618096044",
          "message": "下单使用：订单1618096044",
          "num": -122700,
          "title": "下单使用：订单%s",
          "type": 9
        },
        {
          "date": "2016-04-25 18:56:47",
          "key": "1616262205",
          "message": "下单使用：订单1616262205",
          "num": -41900,
          "title": "下单使用：订单%s",
          "type": 9
        }
      ],
      "limit": 20,
      "page": 1,
      "page_total": 2,
      "total": 2
     },
     "md5": "35a15e3c64140cd2dff1e647fc03f65b",
     "message": "yoho coin list"
    }

 
## 三、acquireMarsPoint()方法：地点签到、合格评价时收入mars点数##
 **请求参数**

| 参数名称 | 参数类型 |  可否为空 | 示例 | 默认值 | 备注 | 
|:-------| -----:|-----:|-----:|-----:|----:| 
|uid|int|否|213|0|用户id|
|num|int|否|100|0|mars点数|
|type|int|否|17或者18|0|地点签到：17，合格评价：18|
|marsId|String|否|123456789|0|关联合格评价的具体评价，关联地点签到的具体地点|
|pid|int|是|100|0|erp操作人员的uid|

**实现逻辑：**

1、YohoCoinLogController.acquireMarsPoint()方法中调用users.acquireMarsPoint服务；

2、对请求参数uid、num、marsId进行非空校验，对参数type进行数值校验，若不为17（"地点签到"）且不为18（"合格评价"），则抛出MARSPOINT_TYPE_INVALID异常；

3、根据请求参数向表yoho_coin_cost和yoho_coin_history中增加mars点数记录（IYohoCoinCostDAO.insertSelective()和IYohoCoinHistoryDAO.insertSelective()）；

4、获取表yoho_coin_cost中的新增记录的id，更新该条记录的logid字段值为该id值；

5、根据用户id查询表yoho_coin_cost，获取该用户的所有有货币num记录，对num求和得该用户当前有货币数目currentCoinNum，返回.

**示例 **

请求：http://localhost:8080/gateway?method=app.marspoint.acquire&debug=XYZ
入口参数body
           {
   			 "uid": 330770,
    		 "num": 100,
             "type": 18,
    		 "marsId":469355,
    		 "pid": 11111111
			}

返回：
        
             {
  			   "alg": "SALT_MD5",
         	   "code": 200,
   			   "data": {
   			   "currency": "400",
   			   "uid": 330770
  			   },
  			   "md5": "0db2249f255e65b4f84f3c07556a0160",
 			   "message": "ok"
			 }


## 三、exchangeMarsPoint()方法：兑换mars点数##
**请求参数**

| 参数名称 | 参数类型 |  可否为空 | 示例 | 默认值 | 备注 | 
|:-------| -----:|-----:|-----:|-----:|----:| 
|uid|int|否|213|0|用户id|
|num|int|否|-100|0|mars点数|
|type|int|否|19、20或21|0|兑换实物：19，兑换话费：20，兑换电子券:21|
|orderCode|String|否|123456789|0|订单号|
|pid|int|是|100|0|erp操作人员的uid|

**实现逻辑：**

1、YohoCoinLogController.exchangeMarsPoint()方法中调用users.exchangeMarsPoint服务；

2、对请求参数uid、num、OrderCode进行非空校验，对参数type进行数值校验，若不为19（"兑换实物"）且不为20（"兑换话费"）且不为22（"兑换电子券"），则抛出MARSPOINT_TYPE_INVALID异常；

3、根据uid查询表yoho_coin_cost，获取该用户的mars点总数(IYohoCoinCostDAO.selectYOHOcoin(uid))，结合请求参数中的num判断mars点数是否足够；

4、 若足够则进行mars点数扣除操作：根据uid查询库表yoho_coin_cost获取用户mars点数根据失效时间排列的记录列表，逐个检查对应的logid的有货券是否够扣；

5、若记录列表中遍历完后，扣除后的用户剩余点数为负数，则抛出当前有货币不足异常；

6、扣除成功后，向表yoho_coin_cost中插入详细扣除记录和向表yoho_coin_history中增加一条扣除总数记录；

7、返回用户当前有货币(mars point)数.

**示例**

请求：http://localhost:8080/gateway?method=app.marspoint.exchange&debug=XYZ

入口参数body

    {
    "uid": 330620,
    "num": -150,
    "type": 20,
    "orderCode": 1234567890,
    "pid": 11111111
    }

返回

     {
      "alg": "SALT_MD5",
      "code": 200,
      "data": {
      "currency": "2850",
      "uid": 330620
     },
     "md5": "74814a916f671e84483dd40bd7741de9",
     "message": "ok"
     }


## 三、returnMarsPoint()方法：mars点数兑换失败返回被使用的mars点数##
**请求参数**

| 参数名称 | 参数类型 |  可否为空 | 示例 | 默认值 | 备注 | 
|:-------| -----:|-----:|-----:|-----:|----:| 
|uid|int|否|213|0|用户id|
|num|int|否|100|0|mars点数|
|type|int|否|22|0|mars点数兑换失败：22|
|orderCode|String|否|123456789|0|订单号|
|pid|int|是|100|0|erp操作人员的uid|

**实现逻辑：**

1、YohoCoinLogController.returnMarsPoint()方法中调用users.returnMarsPoint服务；

2、对请求参数uid、num、OrderCode进行非空校验，对参数type进行数值校验，若不为22（"mars点数兑换失败"），则抛出MARSPOINT_TYPE_INVALID异常；

3、根据uid查询表yoho_coin_cost，获取该用户的mars点总数(IYohoCoinCostDAO.selectYOHOcoin(uid))；

4、根据uid和orderCode查询库表yoho_coin_cost查询表yoho_coin_cost，查询对应的记录有货币数，判断订单使用的yoho币是否都已退还；

5、若存在未退还有货币，根据uid和orderCode查询库表yoho_coin_cost，获取用户的有货币按照logid和过期时间排列的列表；

6、根据列表进行有货币的退还，该过程与有货币扣除过程相反；

7、退还成功后，向表yoho_coin_cost中插入详细退还记录和向表yoho_coin_history中增加一条扣除总数记录；

8、返回用户当前有货币(mars point)数.

**示例**

请求：http://localhost:8080/gateway?method=app.marspoint.return&debug=XYZ

入口参数body

      {
       "uid": 50030199,
       "num": 150,
       "type": 22,
       "orderCode": 1234567890,
       "pid": 11111111
       }

返回：

      {
       "alg": "SALT_MD5",
       "code": 200,
       "data": {
       "currency": "250",
       "uid": 50030199
       },
       "md5": "e79412cd926330de9f49e87046d6c547",
       "message": "ok"
       }








