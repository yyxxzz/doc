# ArticleCommentsController 文章评论相关服务 #
## 1.getArticleCommentsList方法 ##

 -请求参数：
 
| 参数名称 | 参数类型 |  可否为空 | 备注 | 
|:-------| -----:|-----:|-----:|
|article_id|int|否|作者id |
|page|int|否|页数|        
|limit|int|否|返回条数|  


 -方法解释：获取文章评论列表服务。

 -实现逻辑：

    1.进入ArticleCommentsController.getArticleCommentsList方法，调用sns.getArticleCommentsList服务
    2.对参数article_id进行非空和是否大于0校验
    3.根据article_id查询表yh_guang.comments获取该文章的评论信息
    4.调用users.getUserProfilesByUids服务获取评论用户的详细信息
    5.users.getUserProfilesByUids服务中根据uid查询表yoho_passport.user_profile和表yoho_passport.user_base返回用户详细信息
    6.封装数据并返回

 -示例：
 
请求：http://localhost:8080/gateway/guang/api/*/comments/getList?article_id=1&page=1&limit=1&debug=XYZ

返回：
```
{
    "alg": "SALT_MD5",
    "code": 200,
    "data": {
        "list": [
            {
                "article_id": 1,
                "avator": "http://img10.static.yhbimg.com/headimg/2013/11/28/09/01cae078abe5fe320c88cdf4c220212688.gif",
                "content": "帅",
                "create_time": "05月21日 09:42",
                "id": 1,
                "username": "150****4837"
            }
        ],
        "page": 1,
        "total": 1,
        "total_page": 1
    },
    "md5": "a59b5805865c5052f66e9ceafcdef8ca",
    "message": "Comment List!"
}
```


## 2.addArticleComments方法 ##

 -请求参数：
 
| 参数名称 | 参数类型 |  可否为空 | 备注 | 
|:-------| -----:|-----:|-----:|
|article_id|int|否|作者id |
|uid|int|否|评论用户id|        
|content|string|否|评论内容|  

 -方法解释：增加文章评论

 -实现逻辑：

    1.进入ArticleCommentsController.addArticleComments方法，调用sns.addArticleComments服务
    2.对参数进行非空校验
    3.查询表yh_guang.article获取文章详细信息
    4.调用users.getUserProfilesByUids服务获取评论用户的详细信息
    5.组装评论信息插入到yh_guang.comments表中
    6.返回评论成功信息

 -示例：
请求：http://localhost:8080/gateway/guang/api/*/comments/add?article_id=3&uid=1&content=sdfasdfas&debug=XYZ
返回：
```
{
    "alg": "SALT_MD5",
    "code": 200,
    "data": {},
    "md5": "f4a7a490bb6666b005008d795ed14e5d",
    "message": "发表评论成功"
}
```
