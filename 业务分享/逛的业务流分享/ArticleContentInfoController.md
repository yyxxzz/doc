# ArticleContentInfoController获取资讯内容 #
**方法：getArticleContent()**

**请求参数**

| 参数名称 | 参数类型 | 可否为空 |示例  |默认值 |备注  |
| ---------|:--------:| --------:|-----:|------:|-----:|
|article_id|string    | 否       |5     |0      |文章id|
|client_type|string   | 否       |h5    |h5     |客户端类型|

**实现逻辑：**

1、ArticleContentController.sns.getArticleContent()方法中调用sns.guang.sns.getArticleContentForIdClientType服务；

2、对参数article\_id文章id是否为空或是否小于1进行验证，若小于1则抛出文章id文章id错误异常；

3、根据article\_id查询数据库表article获取该文章id对应的文章标题、作者、图片、内容等详细信息，返回Article对象;

4、调用ArticleConvert.toArticleRspBO()方法对Article对象进行转换，返回封装的ArticleRspBO对象；

**示例**
请求：http://localhost:8080/gateway/guang/service/*/article/getArticle?article_id=5&client_type=h5&debug=XYZ

返回：

    {
     "alg": "SALT_MD5",
     "code": 200,
     "data": {
    "article_gender": "3",
    "article_summary": "近年来，阔腿裤因其简单大方的宽松轮廓以及修饰腿型的作用而风靡女装界。",
    "article_title": "谁说阔腿裤是女生专属？",
    "article_type": 1,
    "author_id": 521285,
    "brand": "",
    "browse": 28242,
    "cover_image": "http://img11.static.yhbimg.com/yhb-img01/2015/05/20/21/014578a10fc6007c948f71b977884c0e78.jpg?imageView/{mode}/w/{width}/h/{height}",
    "cover_image_type": 1,
    "create_time": 1432130303,
    "id": 5,
    "is_recommend": 1,
    "max_sort_id": 2,
    "min_sort_id": 0,
    "pageViews": 28249,
    "praise": 25,
    "publishTime": "5月21日 10:13",
    "publish_state": 1,
    "publish_time": 1432174412,
    "status": 1,
    "tag": "阔腿裤,运动",
    "tags": [
      {
        "name": "阔腿裤",
        "url": "http://guang.m.yohobuy.com/tags/index?query=阔腿裤"
      },
      {
        "name": "运动",
        "url": "http://guang.m.yohobuy.com/tags/index?query=运动"
      }
    ],
    "update_time": 1459927151,
    "url": "http://guang.m.yohobuy.com/info/index?id=5"
    },
    "md5": "b049001bf1b2fc6b7ab7e650dbdf05cd",
    "message": "咨询内容"
    }