# ArticleBrandRelatedController  文章相关的品牌信息以及文章内容相关的其他文章 #
## 1.getBrandRelatedByArticle方法 ##

 -请求参数：

| 参数名称 | 参数类型 |  可否为空 | 备注 | 
|:-------| -----:|-----:|-----:|
|article_id|int|否|作者id |
|client_type|string|否|客户端类型|    

 -方法解释：获取文章相关的品牌信息。

 -实现逻辑：

    1.进入ArticleBrandController.getArticleByBrand方法，调用sns.getBrandRelatedByArticle服务
    2.对参数article_id进行非空和是否大于0校验
    3.根据article_id查询表yh_guang.article_brand_relation获取该文章相关的品牌brandIds
    4.根据brandIds调用product.queryBrandByIds服务
    5.product.queryBrandByIds服务中根据brandIds查询表yh_shops.brand获得详细品牌信息
    5.封装数据并返回

 -示例：
请求：localhost:8080/gateway/guang/service/*/article/getBrand?article_id=1&client_type=h5&debug=XYZ

返回：

```
{
    "alg": "SALT_MD5",
    "code": 200,
    "data": [
        {
            "id": "138",
            "name": "THETHING",
            "thumb": "http://img11.static.yhbimg.com/brandLogo/2015/12/09/16/018d25bf4481ed2998bf33b294673a8535.jpg?imageMogr2/thumbnail/100x100/extent/100x100/background/d2hpdGU=/position/center/quality/80",
            "url": "http://thething.m.yohobuy.com"
        },
        {
            "id": "313",
            "name": "Reebok",
            "thumb": "http://img11.static.yhbimg.com/brandLogo/2015/12/09/16/01b024238920cd70d5b276ba37244cfdfe.jpg?imageMogr2/thumbnail/100x100/extent/100x100/background/d2hpdGU=/position/center/quality/80",
            "url": "http://reebok.m.yohobuy.com"
        },
        {
            "id": "397",
            "name": "Pineal Body",
            "thumb": "http://img10.static.yhbimg.com/brandLogo/2015/12/03/15/013b75be7b5b6888aa7935b84abc02276c.jpg?imageMogr2/thumbnail/100x100/extent/100x100/background/d2hpdGU=/position/center/quality/80",
            "url": "http://pinealbody.m.yohobuy.com"
        },
        {
            "id": "518",
            "name": "CLOTtee",
            "thumb": "http://img11.static.yhbimg.com/brandLogo/2015/12/03/15/01de6bf632d371c2d6c5f74c212aa284fb.jpg?imageMogr2/thumbnail/100x100/extent/100x100/background/d2hpdGU=/position/center/quality/80",
            "url": "http://clottee.m.yohobuy.com"
        }
    ],
    "md5": "dc0765a5ea1b974af3db9966a71843ea",
    "message": "文章相关品牌列表"
}
```


## 2.getOtherArticle方法 ##

 - 参数：tags文章标签
	article_id文章id             
         client_type客户端类型
	offset偏移量
	limit返回条数

 -请求参数：

| 参数名称 | 参数类型 |  可否为空 | 备注 | 
|:-------| -----:|-----:|-----:|
|article_id|int|否|作者id |
|client_type|string|否|客户端类型|   
|tags|string|否|文章标签|  
|offset|int|否|偏移量|   
|limit|int|否|返回条数|   


 -方法解释：查询某文章内容的其他推荐文章

 -实现逻辑：

    1.进入ArticleBrandController.getOtherArticle方法，调用sns.getOtherArticle服务
    2.查询表yh_guang.article获取关联文章
    5.封装数据并返回


 -示例：
 
请求：http://localhost:8080/gateway/guang/service/*/article/getOtherArticle?tags=1&article_id=1&client_type=h5&debug=XYZ


返回：
```
{
    "alg": "SALT_MD5",
    "code": 200,
    "data": [
        {
            "cover_image_type": 0,
            "id": 34194,
            "publishTime": "1月1日 08:00",
            "thumb": "http://img13.static.yhbimg.com/article/2016/05/12/13/02a090a95f0835b2c2303006aeda8394a8.jpg?imageView/{mode}/w/{width}/h/{height}",
            "title": "3433",
            "url": "http://guang.m.yohobuy.com/info/index?id=34194"
        },
        {
            "cover_image_type": 0,
            "id": 31626,
            "publishTime": "1月1日 08:00",
            "thumb": "http://img11.static.yhbimg.com/yhb-img01/2016/01/08/03/01aca577c1fbc471f1c76ccaff8f6e293b.jpg?imageView/{mode}/w/{width}/h/{height}",
            "title": "「穿什么看这里」摆脱死气，2016该玩亮色了！",
            "url": "http://guang.m.yohobuy.com/info/index?id=31626"
        },
        {
            "cover_image_type": 1,
            "id": 31240,
            "publishTime": "1月1日 08:00",
            "thumb": "http://img11.static.yhbimg.com/yhb-img01/2016/01/05/08/0178ba5df3aa0980d98ac0908f48cf6ffe.jpg?imageView/{mode}/w/{width}/h/{height}",
            "title": "高性价比MA-1夹克，要潮也不用剁手！",
            "url": "http://guang.m.yohobuy.com/info/index?id=31240"
        },
        {
            "cover_image_type": 1,
            "id": 30748,
            "publishTime": "1月1日 08:00",
            "thumb": "http://img11.static.yhbimg.com/yhb-img01/2015/12/30/07/01669a446d1b1d1a24ac2bea91dc5c5b2e.jpg?imageView/{mode}/w/{width}/h/{height}",
            "title": "2016你要改头换面？那你需要先换掉它！",
            "url": "http://guang.m.yohobuy.com/info/index?id=30748"
        },
        {
            "cover_image_type": 1,
            "id": 28136,
            "publishTime": "1月1日 08:00",
            "thumb": "http://img12.static.yhbimg.com/yhb-img01/2015/12/11/09/0210e01fd064eaa4769b45b869cb74bf98.gif?imageView/{mode}/w/{width}/h/{height}",
            "title": "【年末潮流回顾】2015这些联名一度炸翻潮流圈！",
            "url": "http://guang.m.yohobuy.com/info/index?id=28136"
        },
        {
            "cover_image_type": 0,
            "id": 27626,
            "publishTime": "1月1日 08:00",
            "thumb": "http://img12.static.yhbimg.com/yhb-img01/2015/12/09/08/02a429e4c9500ce61407dd210531c1257f.jpg?imageView/{mode}/w/{width}/h/{height}",
            "title": "加长下摆依然继续！给你层叠的HI—STEET！",
            "url": "http://guang.m.yohobuy.com/info/index?id=27626"
        },
        {
            "cover_image_type": 0,
            "id": 20283,
            "publishTime": "1月1日 08:00",
            "thumb": "http://img11.static.yhbimg.com/yhb-img01/2015/10/21/09/01a18a865f5de0355aa5e72bd68f8b7b5a.jpg?imageView/{mode}/w/{width}/h/{height}",
            "title": "「来自编辑部的私家推荐」话题编辑Naoki闹的MA-1出街造型",
            "url": "http://guang.m.yohobuy.com/info/index?id=20283"
        }
    ],
    "md5": "3aca41942b3b1d40b8810b5ce356f97e",
    "message": "文章相关内容列表"
}
```
