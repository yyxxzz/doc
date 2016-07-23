# ArticlePraiseController  文章点赞相关逻辑 #
## 1.setPraise方法 ##

 -请求参数：
 
| 参数名称 | 参数类型 |  可否为空 | 备注 | 
|:-------| -----:|-----:|-----:|
|article_id|int|否|文章id |
|udid|string|否|设备号|
                
 -方法解释：对文章点赞，并返回点赞数

 -实现逻辑：

    1.进入ArticlePraiseController.setPraise方法，调用sns.setPraise服务
    2.对参数article_id和udid进行非空校验以及对article_id进行是否大于0的校验
    3.根据article_id和udid查询表yh_guang.article_praise得到该设备是否对该文章点过赞
    4.如果没有点过赞，往表yh_guang.article_praise插入这条信息，并增加表yh_guang.article的点赞数量
    5.最后查询表yh_guang.article得到，该文章的点赞数量并返回


 -示例：
 
请求：http://localhost:8080/gateway/guang/api/*/praise/setPraise?article_id=33532&udid=1258e608815a77ba2f9094da936f896ef4fd65721&debug=XYZ

返回：
```
{
    "alg": "SALT_MD5",
    "code": 200,
    "data": 3,
    "md5": "8f9df5102b7c7304f19b994c3e41ad1a",
    "message": "success"
}
```

## 2.cancel方法 ##

 -请求参数：

| 参数名称 | 参数类型 |  可否为空 | 备注 | 
|:-------| -----:|-----:|-----:|
|article_id|int|否|文章id |
|udid|string|否|设备号|

 -方法解释：取消文章点赞

 -实现逻辑：

    1.进入ArticlePraiseController.cancel方法，调用sns.cancelPraise服务
    2.对参数article_id和udid进行非空校验以及对article_id进行是否大于0的校验
    3.根据article_id和udid删除表yh_guang.article_praise
    4.删除表yh_guang.article的点赞数量
    5.最后查询表yh_guang.article得到，该文章的点赞数量并返回


 -示例：
 
请求：http://localhost:8080/gateway/guang/api/*/praise/cancel?article_id=33532&udid=1258e608815a77ba2f9094da936f896ef4fd65721&debug=XYZ

返回：
```
{
    "alg": "SALT_MD5",
    "code": 200,
    "data": 2,
    "md5": "ca6bab969f97634ec41ecab841920e30",
    "message": "success"
}
```
