# ArticleController 逛文章相关信息 #
## 1.getArticleNotice方法 ##

-请求参数：
 
| 参数名称 | 参数类型 |  可否为空 | 备注 | 
|:-------| -----:|-----:|-----:|
|gender|string|否|性别 |
|datetime|string|否|上次提醒时间|


 -方法解释：获取指定时间内发布文章数

 -实现逻辑：

    1.进入ArticleController.getArticleNotice方法，判断datetime是否为空，调用sns.getArticleNotice服务

    2.根据时间和性别查询yh_guang.article表
        
    4.封装对象ArticleNoticeRspBO，然后返回

 -示例：
 
请求：http://127.0.0.1:8080/gateway/guang/api/v1/article/getArticleNotice?app_version=4.1.0.1603120001&client_secret=39dbeae016562ae785a81e4581fe4be7&client_type=iphone&datetime=1453974260.210718&gender=1%2C2%2C3&os_version=9.2.1&screen_size=320x568&udid=258e608815a77ba2f9094da936f896ef4fd65721&v=7&yh_channel=3

返回：
```
 {
        "code": 200,
        "data": {
            "datetime": 1458282983,
            "gender": "1,2,3",
            "total": 19
        },
        "message": "更新数量"
    }
```




## 2.getArticle方法 ##

-请求参数：
 
| 参数名称 | 参数类型 |  可否为空 | 备注 | 
|:-------| -----:|-----:|-----:|
|id|int|否|文章id |



 -方法解释：获取文章内容

 -实现逻辑：

    1.进入ArticleController.getArticle方法，判断id是否为空，调用sns.getArticle服务

    2.根据id查询yh_guang.article表
        
    4.封装对象ArticleRspBO，然后返回

 -示例：
 
请求：http://127.0.0.1:8080/gateway/guang/api/v1/share/guang?app_version=4.1.0.1603120001&client_secret=b78c78d2cac2402e42b7b0af86b743fd&client_type=iphone&id=33533&os_version=9.2.1&screen_size=320x568&v=7&yh_channel=2

返回：
```
 {
        "code": 200,
        "data": {
            "content": "是哈哈哈是是是是哈哈哈是是是是哈哈哈是是是是哈哈哈是是是是哈哈哈是是是是哈哈哈是是是是哈哈哈是是是是哈哈哈是是是是哈哈哈是是是是",
            "pic": "http://img12.static.yhbimg.com/yhb-img01/2016/03/03/06/0232f5ed5a28935ee0c5daf8ee87114629.jpg?imageView/2/w/640/h/640",
            "praise_num": 0,
            "title": "是哈哈哈是是是是哈哈哈是是是是哈哈哈是是是是哈哈哈是是是是哈哈哈是是是是哈哈哈是是是是哈哈哈是是是是哈哈哈是是是是哈哈哈是是是是",
            "url": "http://guang.m.yohobuy.com/info/index?id=33533"
        },
        "message": "文章分享"
    }
```




## 3.getArticleBaseInfo方法 ##

-请求参数：
 
| 参数名称 | 参数类型 |  可否为空 | 备注 | 
|:-------| -----:|-----:|-----:|
|id|int|否|文章id |
|uid|int|否|用户id |
|udid|int|否|设备id |



 -方法解释：获取文章内容详情

 -实现逻辑：

    1.进入ArticleController.getArticleBaseInfo方法，调用sns.getArticleBaseInfo服务

    2.根据id查询yh_guang.article表,获取文章信息
    
    3.查询article_praise和user_favorite获取该用户对该文章的点赞和收藏信息
        
    4.封装对象ArticleRspBO，然后返回

 -示例：
 
请求：http://127.0.0.1:8080/gateway/guang/api/v2/article/getArticleBaseInfo?app_version=4.1.0.1603120001&client_secret=fda947ffa036e19e63ae42d186697f95&client_type=iphone&id=33533&os_version=9.2.1&screen_size=320x568&udid=258e608815a77ba2f9094da936f896ef4fd65721&uid=8039983&v=7

返回：
```
{
        "code": 200,
        "data": {
            "category_id": 4,
            "id": 33533,
            "intro": "435345345345",
            "isFavor": "N",
            "isPraise": "N",
            "is_recommended": 1,
            "praise_num": 0,
            "publish_time": "2016-02-17 10:26:50",
            "src": "http://img12.static.yhbimg.com/yhb-img01/2016/03/03/06/0232f5ed5a28935ee0c5daf8ee87114629.jpg?imageView/{mode}/w/{width}/h/{height}",
            "title": "是哈哈哈是是是是哈哈哈是是是是哈哈哈是是是是哈哈哈是是是是哈哈哈是是是是哈哈哈是是是是哈哈哈是是是是哈哈哈是是是是哈哈哈是是是是",
            "view_num": 18
        },
        "message": "文章分享"
    }
```


## 4.getArticleByViewsNum方法 ##

-请求参数：
 
| 参数名称 | 参数类型 |  可否为空 | 备注 | 
|:-------| -----:|-----:|-----:|
|page|int|否|第几页 |
|limit|int|否|每页条数 |
|gender|string|否|性别 |
|client_type|string|否|客户端类别 |


 -方法解释：获取48小时内浏览最多的文章

 -实现逻辑：

    1.进入ArticleController.getArticleByViewsNum方法，调用sns.getArticleByViewsNum服务

    2.对yh_guang.article表根据browse字段进行降序排序,获取文章信息
           
    3.封装List<ArticleBrowseRspBO>，然后返回

 -示例：
 
请求：http://127.0.0.1:8080/gateway/guang/api/*/article/getArticleByViewsNum?page=1&limit=1&gender=1,3&client_type=h5

返回：
```
{
  "alg": "SALT_MD5",
  "code": 200,
  "data": [
    {
      "ads_img_size": "",
      "article_type": 1,
      "author_id": 8168229,
      "category_id": 3,
      "conver_image_type": 1,
      "id": 33452,
      "intro": "终于迎来了“潮人”的新栏目，潮人并非只是明星idol，潮人就在我们生活中，你的身边。最帅最潮的人都在这，不是明星也是弄潮儿，够潮就来“show”！",
      "is_recommended": 0,
      "praiseStatus": true,
      "praise_num": 0,
      "publish_time": "01月18日 03:58",
      "share_url": {
        "url": "http://guang.m.yohobuy.com/info/index?id=33452&openby:yohobuy={\"action\":\"go.share\",\"params\":{\"pic\":\"http://img11.static.yhbimg.com/yhb-img01/2016/01/19/08/01e853f0ef05dcf84dc5a316c590f4ea56.jpg?imageView/2/w/640/h/640\",\"title\":\"最帅最潮的人都在这，不是明星也是弄潮儿，够潮就来“show”！\",\"content\":\"潮流资讯，新鲜贩售，YOHO!有货【逛】不停\",\"url\":\"http://guang.m.yohobuy.com/info/index?id=33452\"}}"
      },
      "src": "http://img11.static.yhbimg.com/yhb-img01/2016/01/19/08/01e853f0ef05dcf84dc5a316c590f4ea56.jpg?imageView/{mode}/w/{width}/h/{height}",
      "title": "最帅最潮的人都在这，不是明星也是弄潮儿，够潮就来“show”！",
      "url": "http://guang.m.yohobuy.com/info/index?id=33452",
      "views_num": 150
    }
  ],
  "md5": "6806c9559460f374d3cddd6a9c1d6636",
  "message": "获取48小时内浏览最多的文章成功！"
}
```



## 5.getTagTop方法 ##

-请求参数：
 
| 参数名称 | 参数类型 |  可否为空 | 备注 | 
|:-------| -----:|-----:|-----:|
|page|int|否|第几页 |
|limit|int|否|每页条数 |



 -方法解释：获取逛的热门标签

 -实现逻辑：

    1.进入ArticleController.getTagTop方法，调用sns.getArticleByViewsNum服务

    2.查询yh_guang.tags_views表
           
    3.封装List<TagsViewsRspBO>，然后返回

 -示例：
 
请求：http://127.0.0.1:8080/gateway/guang/api/*/article/getTagTop?page=1&limit=2

返回：
```
{
  "alg": "SALT_MD5",
  "code": 200,
  "data": [
    {
      "create_time": 1436959947,
      "id": 9,
      "tag_name": "韩系",
      "views": 96595
    },
    {
      "create_time": 1436959819,
      "id": 3,
      "tag_name": "街头",
      "views": 94789
    }
  ],
  "md5": "af3bb4eb83f81d0f2fc00d036f4c85ae",
  "message": "获取逛的热门标签成功！"
}
```