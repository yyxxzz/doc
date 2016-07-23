# ArticleBrandController根据品牌获取文章信息 #
## 1.getArticleByBrand方法 ##

         
-请求参数：
 
| 参数名称 | 参数类型 |  可否为空 | 备注 | 
|:-------| -----:|-----:|-----:|
|brand_id|int|否|品牌id |
|client_type|string|否|客户端类型|
|limit|int|否|查询文章条数|
|udid|string|否|设备号|


 -方法解释：获取某品牌下的文章信息

 -实现逻辑：

    1.进入ArticleBrandController.getArticleByBrand方法，调用sns.getArticleByBrand服务

    2.对参数brand_id进行非空和是否大于0校验，参数limit如果小于0取3
    
    3.根据brandId查询表yh_guang.article_brand_relation获取该品牌下的文章articleIds
    
    4.根据articleIds查询表yh_guang.article获取文章具体信息articles

    5.根据udid和articleIds查询表yh_guang.article_praise获取文章的点赞列表
 
    6.封装数据并返回

 -示例：
 
请求：http://localhost:8080/gateway/guang/service/*/article/getArticleByBrand?brand_id=3&client_type=h5&udid=8168296&limit=1&debug=XYZ

返回：
```
{
    "alg": "SALT_MD5",
    "code": 200,
    "data": [
        {
            "cover_image_type": "1",
            "id": "34182",
            "intro": "乌拉拉",
            "like": {
                "count": "0",
                "isLiked": false
            },
            "praise_num": "0",
            "publish_time": "5月9日 09:47",
            "share_url": {
                "url": "http://guang.m.yohobuy.com/info/index?id=34182"
            },
            "src": "http://img13.static.yhbimg.com/article/2016/05/09/09/02892d24292d7a90741f4224ade8f089a9.jpg?imageView/{mode}/w/{width}/h/

{height}",
            "title": "添加商品显示问题-汪康",
            "url": "http://guang.m.yohobuy.com/info/index?id=34182",
            "views_num": "479"
        },
        {
            "cover_image_type": "1",
            "id": "34180",
            "intro": "21",
            "like": {
                "count": "0",
                "isLiked": false
            },
            "praise_num": "0",
            "publish_time": "5月7日 17:24",
            "share_url": {
                "url": "http://guang.m.yohobuy.com/info/index?id=34180"
            },
            "src": "http://img10.static.yhbimg.com/article/2016/05/07/17/01f33bd9eb84f3336b7a8ddf7c25f0c8e2.png?imageView/{mode}/w/{width}/h/

{height}",
            "title": "测试多图",
            "url": "http://guang.m.yohobuy.com/info/index?id=34180",
            "views_num": "240"
        },
        {
            "cover_image_type": "2",
            "id": "34178",
            "intro": "据台湾“中央社”5月6日报道，民进党籍“立委”许智杰6日透露称，有台湾学生到土耳其做交换学生，拿到的居留证资料的“国籍栏”被注明为

“中华人民共和国”;台当局涉外部门表示，",
            "like": {
                "count": "0",
                "isLiked": false
            },
            "praise_num": "0",
            "publish_time": "5月7日 15:55",
            "share_url": {
                "url": "http://guang.m.yohobuy.com/info/index?id=34178"
            },
            "src": "http://img11.static.yhbimg.com/article/2016/05/07/15/01bf2eb0977644d52b1ba040cf41269c5f.png?imageView/{mode}/w/{width}/h/

{height}",
            "title": "土耳其将台湾交换生国籍写中华人民共和国",
            "url": "http://news.sohu.com/20160506/n448054944.shtml?=306984407",
            "views_num": "6"
        }
    ],
    "md5": "0dd574c41ce8ea8779a7c218c920c5e4",
    "message": "get article by brand success"
}
```