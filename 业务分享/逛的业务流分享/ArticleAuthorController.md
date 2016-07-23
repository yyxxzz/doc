# ArticleAuthorController获取文章作者信息 #
## 1.getAuthor方法 ##

-请求参数：
 
| 参数名称 | 参数类型 |  可否为空 | 备注 | 
|:-------| -----:|-----:|-----:|
|article_id|int|否|作者id |
|client_type|string|否|客户端类型|


 -方法解释：根据作者id和客户端类型获得作者详细信息

 -实现逻辑：

    1.进入ArticleAuthorController.getAuthor方法，直接调用sns.getArticeAuthor服务

    2.对参数author_id进行非空和是否大于0校验

    3.根据author_id查询表yh_guang.author得到对象author，如果为空抛出作者不存在异常。
    对象author中包含元素：uid作者id，username作者名，avatar头像，authorDesc作者简介，createTime创建时间
        
    4.根据author封装对象ArticleAuthorRspBO，然后返回

 -示例：
 
请求：http://localhost:8080/gateway/guang/service/*/author/getAuthor?author_id=380463&client_type=h5&debug=XYZ

返回：
```
{
    "alg": "SALT_MD5",
    "code": 200,
    "data": {
        "author_desc": "Because i'm fierce，bitch！",
        "avatar": "http://img12.static.yhbimg.com/yhb-img02/2015/06/12/12/02aafe4efc368f307c02b692cf1f862570.jpg?imageView/0/w/100/h/100",
        "name": "uncle-sam",
        "url": "http://guang.m.yohobuy.com/author/index?id=380463"
    },
    "md5": "48e694554ff8e252f7ff59ab09435c83",
    "message": "author info"
}
```