# 一、getArticleContent()方法：获取逛的详情内容 #

**请求参数**

| 参数名称 | 参数类型 | 可否为空 |示例  |默认值 |备注  |
| ---------|:--------:| --------:|-----:|------:|-----:|
|article_id|string    | 否       |5     |0      |文章id|
|client_type|string   | 否       |h5    |h5     |客户端类型|

**实现逻辑：**

1、ArticleContentController.getArticleContent()方法中调用sns.getArticleContent服务；

2、对参数article\_id文章id是否为空或是否小于0进行验证，若为空则抛出文章id不能为空的异常或文章id错误异常；对参数client\_type进行是否为空验证，若为空则设为默认值"h5";

3、根据article\_id查询数据库表article\_block获取该文章id对应的blocks信息，包括详情内容信息和内容键值等信息;

4、对blocks对象进行转换，返回封装的ArticleContentRspBO对象；

**示例**
请求：http://localhost:8080/gateway/guang/service/*/article/getArticleContent?article_id=5&client_type=h5&debug=XYZ

返回：

    {
    "alg": "SALT_MD5",
    "code": 200,
    "data": [
        {
            "singleImage": {
                "data": [
                    {
                        "alt": "",
                        "imgId": "0",
                        "src": "http://img11.static.yhbimg.com/yhb-img01/2015/05/20/21/01814149701999001dfbcfdb330faa835c.jpg?imageView/{mode}/w/{width}/h/{height}",
                        "title": "",
                        "url": ""
                    }
                ],
                "template_intro": "一张图片",
                "template_name": "single_image"
            }
        },
        {
            "text": {
                "data": {
                    "text": "近年来，阔腿裤因其简单大方的宽松轮廓以及修饰腿型的作用而风靡女装界。没有瘦腿裤那样的束缚感，其舒适的穿着体验让它无论是休闲还是工作都可以轻松胜任。如果男生选择阔腿裤，则可以相应的选择一件线条硬朗的牛仔外套来中和阔腿裤的阴柔气质，同时更能提升整体的复古气质。"
                },
                "template_name": "text"
            }
        },
        {
            "goods": {
                "data": [
                    {
                        "id": "51089154",
                        "src": "http://img12.static.yhbimg.com/goodsimg/2015/04/08/04/02bea7e4264fb50b359d58b42d4a697d9c.jpg?imageMogr2/thumbnail/{width}x{height}/extent/{width}x{height}/background/d2hpdGU=/position/center/quality/90"
                    },
                    {
                        "id": "51099177",
                        "src": "http://img13.static.yhbimg.com/goodsimg/2015/04/23/08/02bbf846531154372380001c16ce7e02ed.jpg?imageMogr2/thumbnail/{width}x{height}/extent/{width}x{height}/background/d2hpdGU=/position/center/quality/90"
                    },
                    {
                        "id": "51084637",
                        "src": "http://img11.static.yhbimg.com/goodsimg/2015/01/27/18/014f3a8f45569be08e96d1cff644bcf0bf.jpg?imageMogr2/thumbnail/{width}x{height}/extent/{width}x{height}/background/d2hpdGU=/position/center/quality/90"
                    },
                    {
                        "id": "51006271",
                        "src": "http://img10.static.yhbimg.com/goodsimg/2014/07/09/05/015ca75647a5db9a58bae538a1212b0009.jpg?imageMogr2/thumbnail/{width}x{height}/extent/{width}x{height}/background/d2hpdGU=/position/center/quality/90"
                    },
                    {
                        "id": "51089332",
                        "src": "http://img12.static.yhbimg.com/goodsimg/2015/04/01/09/02f3fc94d79d86957d1ae3c43bf560c83a.jpg?imageMogr2/thumbnail/{width}x{height}/extent/{width}x{height}/background/d2hpdGU=/position/center/quality/90"
                    },
                    {
                        "id": "51082653",
                        "src": "http://img12.static.yhbimg.com/goodsimg/2015/04/10/06/02fecd43129f5322c4df29a723d62ed0af.jpg?imageMogr2/thumbnail/{width}x{height}/extent/{width}x{height}/background/d2hpdGU=/position/center/quality/90"
                    },
                    {
                        "id": "51074098",
                        "src": "http://img11.static.yhbimg.com/goodsimg/2014/10/21/07/014a48606fdbbba5c834ccfdd1ef833e98.jpg?imageMogr2/thumbnail/{width}x{height}/extent/{width}x{height}/background/d2hpdGU=/position/center/quality/90"
                    }
                ],
                "template_intro": "添加商品",
                "template_name": "goods"
            }
        }
    ],
    "md5": "2bdb1055a60ba4663772544a157434dd",
    "message": "get article content success"
    }


# 二、checkArticleFav()方法：判断用户是否收藏逛的文章 #

**请求参数**

| 参数名称 | 参数类型 | 可否为空 |示例  |默认值 |备注  |
| ---------|:--------:| --------:|-----:|------:|-----:|
|article_id|string    | 否       |35    |0      |文章id|
|uid       |string   | 否       |5324120|否    |用户id|
  
**实现逻辑：**

1、ArticleContentController.checkArticleFav()方法中调用sns.checkArticleFav服务；

2、对参数article\_id文章id是否为空或是否小于0进行验证，若为空则抛出文章id不能为空的异常或文章id错误异常；对uid进行如上同样的验证；

3、根据article\_id和uid查询数据库表user\_favorite,得到UserFavorite对象，包括用户id、文章id、记录创建时间等信息;

4、判断UserFavorite对象对象是否为空，为空则表示user\_favorite表中没有该条记录，即该用户未收藏该文章，返回false；不为空，则表示存在该article\_id和uid对应的一条记录，返回true.

**示例**
请求： http://localhost:8080/gateway/guang/service/*/article/checkArticleFav?article_id=35&uid=5357305&debug=XYZ

返回：
   
    {
     "alg": "SALT_MD5",
     "code": 200,
     "data": false,
     "md5": "1073b2b29ffc3b2ada0de06b683382a6",
    "message": "操作成功"
     }
