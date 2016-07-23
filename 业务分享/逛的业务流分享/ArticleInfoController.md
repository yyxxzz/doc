# ArticleInfoController #
## 一、getCategoryList()方法：获取分类下的资讯列表 ##
 **请求参数**

| 参数名称 | 参数类型 | 可否为空 |示例  |默认值 |备注  |
| ---------|:--------:| --------:|-----:|------:|-----:|
|client_type|string    | 否       |h5   |h5   |客户端类型|
|gender|string  | 否       |1    |-     |性别|

**实现逻辑：**

1、ArticleInfoController.getCategoryList()方法中调用sns.getCategory服务；

2、进行分类列表查询前进行查询条件的组装，设置父目录的id：ParentId=0，设置查询的列表状态条件为启用状态：Status=1；

3、在查询前校验请求参数ParentId和Status，若不为空，则列入查询条件，根据组装的查询条件对象ArticleSort查询数据库表article_sort，获取文章分类列表articleSortList；

4、若查询结果articleSortList不为空，则获取列表中的id和name信息，返回封装的categoryRspBOList对象.


**示例：**

请求：
http://localhost:8080/gateway/guang/api/*/category/get?client_type=h5&gender=1&debug=XYZ

返回：
   
     {
     "alg": "SALT_MD5",
     "code": 200,
     "data": [
    {
      "id": "0",
      "name": "推荐"
    },
    {
      "id": "1",
      "name": "话题"
    },
    {
      "id": "2",
      "name": "搭配"
    },
    {
      "id": "3",
      "name": "潮人"
    },
    {
      "id": "4",
      "name": "潮品"
    },
    {
      "id": "59",
      "name": "专题"
    }
    ],
    "md5": "194e510756289bb8f83456c88373fdc1",
    "message": "分类列表"
    }

## 二、getArticleList()方法：获取分类下的文章列表 ##

** 请求参数**

| 参数名称 | 参数类型 | 可否为空 |示例  |默认值 |备注  |
| ---------|:--------:| --------:|-----:|------:|-----:|
|sort_id|string|是|-|0|分类id|
|gender|string|是|emptyString|-|性别|
|author_id|string|是|1|0|作者id|
|tag|string|是|-|emptyString|标签|
|page|string|是|1|1|页码|
|uid|string|是|25|0|用户id|
|udid|string|是|-|emptyString|设备id|
|limit|string|是|10|10|分页大小|
|client_type|string|是|iphone|-|客户端类型|	 

**实现逻辑：**

1、ArticleInfoControllergetArticleList()方法中调用sns.getList服务；

2、进行分类列表查询前进行请求参数的处理，根据请求参数查询数据库表article获取文章的数量；

3、查询数据库表article_sort，获取所有的文章分类列表articleSortList；

4、根据请求参数中的页码page信息从数据库表article中分页查询咨询列表，得查询结果列表articles；

5、从查询结果articles中分别获取所有的文章id列表articleIdList和作者列表信息authorIdList；

6、根据articleIdList列表信息查询数据库表comments，获取文章的评论数量列表信息articleCommentsCountMap；根据authorIdList列表信息获取作者的信息列表MAP：authorMap；

7、根据请求参数中的设备id(UDid)查询库表article_praise，获取文章的点赞列表articlePraises，转换为articlePraiseMap；

8、根据请求参数中的用户id(Uid)查询数据库表user_favorite获取该用户的收藏列表信息userFavoriteMap；

9、通过buildArticleInfo()方法组装所有文章信息，返回articleInfoBOList；

10、根据请求参数中的标签tag查询数据库表tags_views，若存在该条浏览记录，则将该表的views字段值加1；若不存在，则在该表中增加该条浏览记录；

11、组装相应参数，返回对象articleInfoRspBO.

**示例：**

请求：
http://localhost:8080/gateway/guang/api/*/article/getList?debug=XYZ

返回：
    
     {
    "alg": "SALT_MD5",
    "code": 200,
    "data": {
    "datetime": "1463123061",
    "list": {
      "adlist": [
        {
          "bgColor": "",
          "src": "http://img12.static.yhbimg.com/yhb-img01/2016/04/18/14/0241a5f823e05c0c4975b346ed5a3c8a1a.jpg?imageView2/{mode}/w/{width}/h/{height}",
          "title": "",
          "url": "http://feature.yoho.cn/0418RUNNINGMAN/index.html?title=兄弟在奔跑，潮流在有货&share_id=944"
        },
        {
          "bgColor": "",
          "src": "http://img11.static.yhbimg.com/yhb-img01/2016/04/19/01/0183769e29a4a77462ee276bbd5a94787f.jpg?imageView2/{mode}/w/{width}/h/{height}",
          "title": "",
          "url": "http://feature.yohobuy.com/0/0/897/index.html?title=我的潮流世界观&share_id=942"
        },
        {
          "bgColor": "",
          "src": "http://img11.static.yhbimg.com/yhb-img01/2016/04/18/10/01800e18e1db50caaef5486f6355e57bb9.jpg?imageView2/{mode}/w/{width}/h/{height}",
          "title": "",
          "url": "http://feature.yoho.cn/0418APPLIST/index.html?title=带你回顾时尚大电影&share_id=940"
        },
        {
          "bgColor": "",
          "src": "http://img12.static.yhbimg.com/yhb-img01/2016/04/11/10/027feebcf981dd2cc3fb08c610746bec30.jpg?imageView2/{mode}/w/{width}/h/{height}",
          "title": "",
          "url": "http://feature.yohobuy.com/0/0/896/index.html?title=潮流清单&share_id=918"
        },
        {
          "bgColor": "",
          "src": "http://img12.static.yhbimg.com/yhb-img01/2016/04/14/02/02b49e9bbf24f9bd403a95aed83c8ae064.jpg?imageView2/{mode}/w/{width}/h/{height}",
          "title": "",
          "url": "http://feature.yoho.cn/NEWS/0414APPWEEKNEWSALL/index.html?title=一周速报&share_id=912"
        },
        {
          "bgColor": "",
          "src": "http://img12.static.yhbimg.com/yhb-img01/2016/04/11/05/02579a7e14db7c17872a7deb4a8a31688a.jpg?imageView2/{mode}/w/{width}/h/{height}",
          "title": "",
          "url": "http://feature.yoho.cn/0411SNEAKERHEAD/index.html?title=SNEAKERHEAD REPORT&share_id=920"
        },
        {
          "bgColor": "",
          "src": "http://img11.static.yhbimg.com/yhb-img01/2016/04/08/17/01ab07730fbfae2de97403e480f18b4239.jpg?imageView2/{mode}/w/{width}/h/{height}",
          "title": "",
          "url": "http://feature.yoho.cn/0408APPSEASON/index.html?title=机能穿衣舒适又加分&share_id=916"
        },
        {
          "bgColor": "",
          "src": "http://img12.static.yhbimg.com/yhb-img01/2016/04/12/07/02aef74138f1265126fdd76f8a42dfca5e.jpg?imageView2/{mode}/w/{width}/h/{height}",
          "title": "",
          "url": "http://guang.m.yohobuy.com/info/index?id=42776"
        },
        {
          "bgColor": "",
          "src": "http://img11.static.yhbimg.com/yhb-img01/2016/04/12/07/01dd0bee9513a94286ecbd0dec911865f7.jpg?imageView2/{mode}/w/{width}/h/{height}",
          "title": "",
          "url": "http://guang.m.yohobuy.com/info/index?id=42774"
        },
        {
          "bgColor": "",
          "src": "http://img12.static.yhbimg.com/yhb-img01/2016/04/08/03/02186b0b66cdc37db1b9d2c3181b4f1bde.jpg?imageView2/{mode}/w/{width}/h/{height}",
          "title": "",
          "url": "http://feature.yohobuy.com/0/0/895/index.html?title=宅男女神们的小秘密&share_id=914"
        },
        {
          "bgColor": "",
          "src": "http://img12.static.yhbimg.com/yhb-img01/2016/03/21/03/02d683f021eb6c21a2f19b7ddce5caa63a.jpg?imageView2/{mode}/w/{width}/h/{height}",
          "title": "",
          "url": "http://m.yohobuy.com/?title=星潮教室&content_code=ad28931ba6e5ff22ee79eb41943d77d7&template_id=60"
        }
      ],
      "artList": [
        {
          "article_type": "1",
          "author": {
            "author_id": "8168294",
            "avatar": "http://img12.static.yhbimg.com/yhb-img02/2015/06/12/14/0220208c13de341448647b7eead758d592.jpg?imageView/0/w/100/h/100",
            "name": "1",
            "url": "http://guang.m.yohobuy.com/author/index?id=8168294"
          },
          "author_id": "8168294",
          "category_id": "19",
          "category_name": "",
          "conver_image_type": "1",
          "id": 34192,
          "intro": "45",
          "isFavor": "N",
          "isPraise": "N",
          "is_recommended": "1",
          "praiseStatus": "true",
          "praise_num": "0",
          "publish_time": "05月12日 13:39",
          "share": {
            "url": "http://guang.m.yohobuy.com/info/index?id=34192"
          },
          "src": "http://img12.static.yhbimg.com/article/2016/05/11/14/020c0f2b076e80abf1a92a121ad02c3327.jpg?imageView/{mode}/w/{width}/h/{height}",
          "title": "1111",
          "url": "http://guang.m.yohobuy.com/info/index?id=34192",
          "views_num": "134"
        },
        {
          "article_type": "1",
          "author": {
            "author_id": "8168296",
            "avatar": "http://img10.static.yhbimg.com/author/2016/05/07/15/01bb3ae789c573502830726c3297d0c80a.png",
            "name": "雾萌萌",
            "url": "http://guang.m.yohobuy.com/author/index?id=8168296"
          },
          "author_id": "8168296",
          "category_id": "1",
          "category_name": "话题",
          "conver_image_type": "1",
          "id": 34190,
          "intro": "恩恩",
          "isFavor": "N",
          "isPraise": "N",
          "is_recommended": "1",
          "praiseStatus": "true",
          "praise_num": "0",
          "publish_time": "05月10日 19:30",
          "share": {
            "url": "http://guang.m.yohobuy.com/info/index?id=34190"
          },
          "src": "http://img13.static.yhbimg.com/article/2016/05/10/18/021ee5af0d9b4471e27626945f5f59ef67.jpg?imageView/{mode}/w/{width}/h/{height}",
          "title": "测试",
          "url": "http://guang.m.yohobuy.com/info/index?id=34190",
          "views_num": "76"
        },
        {
          "article_type": "1",
          "author": {
            "author_id": "589238",
            "avatar": "http://img12.static.yhbimg.com/yhb-img02/2015/06/12/12/029c6f3c1e63043a13e895c2a861528d3d.jpg?imageView/0/w/100/h/100",
            "name": "三花一木",
            "url": "http://guang.m.yohobuy.com/author/index?id=589238"
          },
          "author_id": "589238",
          "category_id": "19",
          "category_name": "",
          "conver_image_type": "1",
          "id": 34188,
          "intro": "锁定",
          "isFavor": "N",
          "isPraise": "N",
          "is_recommended": "1",
          "praiseStatus": "true",
          "praise_num": "0",
          "publish_time": "05月10日 18:50",
          "share": {
            "url": "http://guang.m.yohobuy.com/info/index?id=34188"
          },
          "src": "http://img11.static.yhbimg.com/article/2016/05/10/18/01d27e1844bfce850bf13c6b4e67c78547.jpg?imageView/{mode}/w/{width}/h/{height}",
          "title": "11",
          "url": "http://guang.m.yohobuy.com/info/index?id=34188",
          "views_num": "524"
        },
        {
          "article_type": "1",
          "author": {
            "author_id": "589238",
            "avatar": "http://img12.static.yhbimg.com/yhb-img02/2015/06/12/12/029c6f3c1e63043a13e895c2a861528d3d.jpg?imageView/0/w/100/h/100",
            "name": "三花一木",
            "url": "http://guang.m.yohobuy.com/author/index?id=589238"
          },
          "author_id": "589238",
          "category_id": "3",
          "category_name": "潮人",
          "conver_image_type": "1",
          "id": 34184,
          "intro": "地方飒飒的",
          "isFavor": "N",
          "isPraise": "N",
          "is_recommended": "1",
          "praiseStatus": "true",
          "praise_num": "1",
          "publish_time": "05月10日 17:30",
          "share": {
            "url": "http://guang.m.yohobuy.com/info/index?id=34184"
          },
          "src": "http://img12.static.yhbimg.com/article/2016/05/09/11/0270c89cc4a789a825cf4f6ecc02789e2e.gif?imageView/{mode}/w/{width}/h/{height}",
          "title": "代码显示检查-汪康",
          "url": "http://guang.m.yohobuy.com/info/index?id=34184",
          "views_num": "952"
        },
        {
          "article_type": "1",
          "author": {
            "author_id": "8168292",
            "avatar": "http://img13.static.yhbimg.com/author/2016/04/26/11/025bce88d3e2f071cfed867cd0cc1322ce.jpg",
            "name": "missing you",
            "url": "http://guang.m.yohobuy.com/author/index?id=8168292"
          },
          "author_id": "8168292",
          "category_id": "2",
          "category_name": "搭配",
          "conver_image_type": "1",
          "id": 34174,
          "intro": "missing you",
          "isFavor": "N",
          "isPraise": "N",
          "is_recommended": "1",
          "praiseStatus": "true",
          "praise_num": "0",
          "publish_time": "05月10日 09:10",
          "share": {
            "url": "http://guang.m.yohobuy.com/info/index?id=34174"
          },
          "src": "http://img11.static.yhbimg.com/article/2016/04/29/10/01bb19f6bb0af56e0113c9ebff87316243.jpg?imageView/{mode}/w/{width}/h/{height}",
          "title": "保存文本验收",
          "url": "http://guang.m.yohobuy.com/info/index?id=34174",
          "views_num": "80"
        },
        {
          "article_type": "1",
          "author": {
            "author_id": "8168290",
            "avatar": "http://img13.static.yhbimg.com/author/2016/04/25/17/02718717297ec254697aaf0a614dd72d8a.jpg",
            "name": "梁边妖",
            "url": "http://guang.m.yohobuy.com/author/index?id=8168290"
          },
          "author_id": "8168290",
          "category_id": "1",
          "category_name": "话题",
          "conver_image_type": "1",
          "id": 34186,
          "intro": "111122200",
          "isFavor": "N",
          "isPraise": "N",
          "is_recommended": "1",
          "praiseStatus": "true",
          "praise_num": "0",
          "publish_time": "05月09日 14:41",
          "share": {
            "url": "http://guang.m.yohobuy.com/info/index?id=34186"
          },
          "src": "http://img12.static.yhbimg.com/article/2016/05/09/14/0201920ab28fc0e52d95622bcc2d0e9ce3.jpg?imageView/{mode}/w/{width}/h/{height}",
          "title": "FDFSD",
          "url": "http://guang.m.yohobuy.com/info/index?id=34186",
          "views_num": "60"
        },
        {
          "article_type": "1",
          "author": {
            "author_id": "8168290",
            "avatar": "http://img13.static.yhbimg.com/author/2016/04/25/17/02718717297ec254697aaf0a614dd72d8a.jpg",
            "name": "梁边妖",
            "url": "http://guang.m.yohobuy.com/author/index?id=8168290"
          },
          "author_id": "8168290",
          "category_id": "1",
          "category_name": "话题",
          "conver_image_type": "1",
          "id": 34182,
          "intro": "乌拉拉",
          "isFavor": "N",
          "isPraise": "N",
          "is_recommended": "1",
          "praiseStatus": "true",
          "praise_num": "0",
          "publish_time": "05月09日 09:47",
          "share": {
            "url": "http://guang.m.yohobuy.com/info/index?id=34182"
          },
          "src": "http://img13.static.yhbimg.com/article/2016/05/09/09/02892d24292d7a90741f4224ade8f089a9.jpg?imageView/{mode}/w/{width}/h/{height}",
          "title": "添加商品显示问题-汪康",
          "url": "http://guang.m.yohobuy.com/info/index?id=34182",
          "views_num": "491"
        },
        {
          "article_type": "1",
          "author": {
            "author_id": "8168296",
            "avatar": "http://img10.static.yhbimg.com/author/2016/05/07/15/01bb3ae789c573502830726c3297d0c80a.png",
            "name": "雾萌萌",
            "url": "http://guang.m.yohobuy.com/author/index?id=8168296"
          },
          "author_id": "8168296",
          "category_id": "1",
          "category_name": "话题",
          "conver_image_type": "1",
          "id": 34180,
          "intro": "21",
          "isFavor": "N",
          "isPraise": "N",
          "is_recommended": "1",
          "praiseStatus": "true",
          "praise_num": "0",
          "publish_time": "05月07日 17:24",
          "share": {
            "url": "http://guang.m.yohobuy.com/info/index?id=34180"
          },
          "src": "http://img10.static.yhbimg.com/article/2016/05/07/17/01f33bd9eb84f3336b7a8ddf7c25f0c8e2.png?imageView/{mode}/w/{width}/h/{height}",
          "title": "测试多图",
          "url": "http://guang.m.yohobuy.com/info/index?id=34180",
          "views_num": "253"
        },
        {
          "ads_img_size": "1088680",
          "article_type": "1",
          "author": {
            "author_id": "8168296",
            "avatar": "http://img10.static.yhbimg.com/author/2016/05/07/15/01bb3ae789c573502830726c3297d0c80a.png",
            "name": "雾萌萌",
            "url": "http://guang.m.yohobuy.com/author/index?id=8168296"
          },
          "author_id": "8168296",
          "category_id": "3",
          "category_name": "潮人",
          "conver_image_type": "2",
          "id": 34178,
          "intro": "据台湾“中央社”5月6日报道，民进党籍“立委”许智杰6日透露称，有台湾学生到土耳其做交换学生，拿到的居留证资料的“国籍栏”被注明为“中华人民共和国”;台当局涉外部门表示，",
          "isFavor": "N",
          "isPraise": "N",
          "is_recommended": "1",
          "praiseStatus": "true",
          "praise_num": "0",
          "publish_time": "05月07日 15:55",
          "share": {
            "url": "http://guang.m.yohobuy.com/info/index?id=34178"
          },
          "src": "http://img11.static.yhbimg.com/article/2016/05/07/15/01bf2eb0977644d52b1ba040cf41269c5f.png?imageView/{mode}/w/{width}/h/{height}",
          "title": "土耳其将台湾交换生国籍写中华人民共和国",
          "url": "http://news.sohu.com/20160506/n448054944.shtml?=306984407",
          "views_num": "6"
        },
        {
          "article_type": "1",
          "author": {
            "author_id": "589238",
            "avatar": "http://img12.static.yhbimg.com/yhb-img02/2015/06/12/12/029c6f3c1e63043a13e895c2a861528d3d.jpg?imageView/0/w/100/h/100",
            "name": "三花一木",
            "url": "http://guang.m.yohobuy.com/author/index?id=589238"
          },
          "author_id": "589238",
          "category_id": "1",
          "category_name": "话题",
          "conver_image_type": "1",
          "id": 34176,
          "intro": "添加内容测试添加内容测试添加内容测试添加内容测试添加内容测试添加内容测试",
          "isFavor": "N",
          "isPraise": "N",
          "is_recommended": "1",
          "praiseStatus": "true",
          "praise_num": "0",
          "publish_time": "05月07日 14:41",
          "share": {
            "url": "http://guang.m.yohobuy.com/info/index?id=34176"
          },
          "src": "http://img10.static.yhbimg.com/article/2016/05/07/14/01a5fd1909a45e4dc028bd52e817a7cada.png?imageView/{mode}/w/{width}/h/{height}",
          "title": "一鞋多拥 足下工业 foot industry Lesiurely Weave CINNAMON STICK",
          "url": "2436432643",
          "views_num": "5"
        }
      ]
    },
    "page": 1,
    "total": 60,
    "totalPage": 6
    },
    "md5": "17bd8f98b5ea8ba2c6c4a13c3dc7bfa4",
    "message": "资讯列表"
    } 

 
## 三、getStarClassroomArticleList()方法：获得‘星潮教室’一级分类中，标签开启状态下的文章 ## 

 **请求参数**

| 参数名称 | 参数类型 | 可否为空 |示例  |默认值 |备注  |
| ---------|:--------:| --------:|-----:|------:|-----:|
|sort_id|string|是|-|0|分类id|
|gender|string|是|emptyString|-|性别|
|author_id|string|是|1|0|作者id|
|tag|string|是|-|emptyString|标签|
|page|string|是|1|1|页码|
|uid|string|是|25|0|用户id|
|udid|string|是|-|emptyString|设备id|
|limit|string|是|10|10|分页大小|
|client_type|string|是|iphone|-|客户端类型|

**实现逻辑：**

1、ArticleInfoController.getStarClassroomArticleList()方法中调用sns.getStarClassroomArticleList服务，该请求参数中的client_type置为“android”；

2、查询数据库表article_tags获得“星潮教室”一级分类中所有子分类的标签名称集合tags ；

3、查询库表article_sort，获取所有文章分类的列表信息articleSortMap；

4、查询库表article，获取“星潮教室”子分类开启状态标签下的文件列表articles；

5、从articles中获取文章的ID列表articleIdList和文章的作者列表authorIdList；

6、根据authorIdList查询库表author，获取作者信息authorMap；

7、根据articleIdList列表信息查询数据库表comments，获取文章的评论数量列表信息articleCommentsCountMap；根据authorIdList列表信息获取作者的信息列表MAP：authorMap；

8、根据请求参数中的标签tag查询数据库表tags_views，若存在该条浏览记录，则将该表的views字段值加1；若不存在，则在该表中增加该条浏览记录；

9、组装响应参数，返回对象articleInfoRspBO.


**示例：**

请求：
http://localhost:8080/gateway/guang/api/*/article/getStarClassroomArticleList?debug=XYZ

返回：
 
     {
    "alg": "SALT_MD5",
    "code": 200,
    "data": {
    "datetime": "1463130182",
    "list": {
      "artList": [
        {
          "article_type": "1",
          "author": {
            "author_id": "7304719",
            "avatar": "http://img12.static.yhbimg.com/yhb-img02/2015/06/12/11/02ad0fa0a6066ede986b969840c620540f.jpg?imageView/0/w/100/h/100",
            "name": "胡子小鸡",
            "url": "http://guang.m.yohobuy.com/author/index?id=7304719&openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"7304719\"},\"share\":\"\",\"id\":7304719,\"type\":0,\"islogin\":\"N\",\"url\":\"http://guang.m.yohobuy.com/author/index\"}}"
          },
          "author_id": "7304719",
          "category_id": "19",
          "category_name": "",
          "conver_image_type": "1",
          "id": 34172,
          "intro": "添加商品测试",
          "isFavor": "N",
          "isPraise": "N",
          "is_recommended": "1",
          "praiseStatus": "true",
          "praise_num": "0",
          "publish_time": "06月22日 14:25",
          "share": {
            "url": "http://guang.m.yohobuy.com/info/index?id=34172&openby:yohobuy={\"action\":\"go.share\",\"params\":{\"pic\":\"http://img13.static.yhbimg.com/article/2016/04/28/17/027f1fbd543afb82d95047b0c578010aa0.png?imageView/2/w/640/h/640\",\"title\":\"是的\",\"url\":\"http://guang.m.yohobuy.com/info/index?id=34172\",\"content\":\"潮流资讯，新鲜贩售，YOHO!有货【逛】不停\"}}"
          },
          "src": "http://img13.static.yhbimg.com/article/2016/04/28/17/027f1fbd543afb82d95047b0c578010aa0.png?imageView/{mode}/w/{width}/h/{height}",
          "title": "是的",
          "url": "http://guang.m.yohobuy.com/info/index?id=34172&openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"34172\"},\"update_flag\":\"3e72e73226c8c5cd3d34e84270933fae\",\"shareparam\":{\"id\":34172},\"share\":\"/guang/api/v1/share/guang\",\"id\":34172,\"type\":1,\"url\":\"http://guang.m.yohobuy.com/info/index\",\"islogin\":\"N\"}}",
          "views_num": "542"
        },
        {
          "ads_img_size": "1088680",
          "article_type": "1",
          "author": {
            "author_id": "8168296",
            "avatar": "http://img10.static.yhbimg.com/author/2016/05/07/15/01bb3ae789c573502830726c3297d0c80a.png",
            "name": "雾萌萌",
            "url": "http://guang.m.yohobuy.com/author/index?id=8168296&openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"8168296\"},\"share\":\"\",\"id\":8168296,\"type\":0,\"islogin\":\"N\",\"url\":\"http://guang.m.yohobuy.com/author/index\"}}"
          },
          "author_id": "8168296",
          "category_id": "3",
          "category_name": "潮人",
          "conver_image_type": "2",
          "id": 34178,
          "intro": "据台湾“中央社”5月6日报道，民进党籍“立委”许智杰6日透露称，有台湾学生到土耳其做交换学生，拿到的居留证资料的“国籍栏”被注明为“中华人民共和国”;台当局涉外部门表示，",
          "isFavor": "N",
          "isPraise": "N",
          "is_recommended": "1",
          "praiseStatus": "true",
          "praise_num": "0",
          "publish_time": "05月07日 15:55",
          "share": {
            "url": "http://guang.m.yohobuy.com/info/index?id=34178&openby:yohobuy={\"action\":\"go.share\",\"params\":{\"pic\":\"http://img11.static.yhbimg.com/article/2016/05/07/15/01bf2eb0977644d52b1ba040cf41269c5f.png?imageView/2/w/640/h/640\",\"title\":\"土耳其将台湾交换生国籍写中华人民共和国\",\"url\":\"http://guang.m.yohobuy.com/info/index?id=34178\",\"content\":\"潮流资讯，新鲜贩售，YOHO!有货【逛】不停\"}}"
          },
          "src": "http://img11.static.yhbimg.com/article/2016/05/07/15/01bf2eb0977644d52b1ba040cf41269c5f.png?imageView/{mode}/w/{width}/h/{height}",
          "title": "土耳其将台湾交换生国籍写中华人民共和国",
          "url": "http://news.sohu.com/20160506/n448054944.shtml?=306984407&openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"\":\"306984407\",\"url\":\"http://news.sohu.com/20160506/n448054944.shtml\"}}",
          "views_num": "6"
        },
        {
          "ads_img_size": "1088680",
          "article_type": "1",
          "author_id": "521285",
          "category_id": "2",
          "category_name": "搭配",
          "conver_image_type": "1",
          "id": 3,
          "intro": "依稀记得上初中那会，谁要有一件胸口带“钩子”的衣服，那他/她走路一定昂首挺胸，生怕别人看不见一样！",
          "isFavor": "N",
          "isPraise": "N",
          "is_recommended": "0",
          "praiseStatus": "true",
          "praise_num": "28",
          "publish_time": "03月30日 00:00",
          "share": {
            "url": "http://guang.m.yohobuy.com/info/index?id=3&openby:yohobuy={\"action\":\"go.share\",\"params\":{\"pic\":\"http://img10.static.yhbimg.com/yhb-img01/2015/05/20/20/017f40fe5be35c9e1412cd8e2164c87e83.jpg?imageView/2/w/640/h/640\",\"title\":\"会动的的免费广告！\",\"url\":\"http://guang.m.yohobuy.com/info/index?id=3\",\"content\":\"潮流资讯，新鲜贩售，YOHO!有货【逛】不停\"}}"
          },
          "src": "http://img10.static.yhbimg.com/yhb-img01/2015/05/20/20/017f40fe5be35c9e1412cd8e2164c87e83.jpg?imageView/{mode}/w/{width}/h/{height}",
          "title": "会动的的免费广告！",
          "url": "asdasdas?openby:yohobuy={\"action\":\"go.mine\"}",
          "views_num": "39859"
        },
        {
          "ads_img_size": "",
          "article_type": "1",
          "author_id": "5105041",
          "category_id": "1",
          "category_name": "话题",
          "conver_image_type": "2",
          "id": 29630,
          "intro": "Wuli凡凡前天回广州老家，小蛮腰可是为他亮起了欢迎语~这阵仗，简直了。看来《老炮儿》和wuli凡凡的人气，实在是高的不要不要。~",
          "isFavor": "N",
          "isPraise": "N",
          "is_recommended": "0",
          "praiseStatus": "true",
          "praise_num": "49",
          "publish_time": "12月25日 22:00",
          "share": {
            "url": "http://guang.m.yohobuy.com/info/index?id=29630&openby:yohobuy={\"action\":\"go.share\",\"params\":{\"pic\":\"http://img11.static.yhbimg.com/yhb-img01/2015/12/25/10/014df3a77aad110a75f4c63865461785da.jpg?imageView/2/w/640/h/640\",\"title\":\"《老炮儿》造型大吐槽！吴亦凡李易峰戏里戏外造型PK！\",\"url\":\"http://guang.m.yohobuy.com/info/index?id=29630\",\"content\":\"潮流资讯，新鲜贩售，YOHO!有货【逛】不停\"}}"
          },
          "src": "http://img11.static.yhbimg.com/yhb-img01/2015/12/25/10/014df3a77aad110a75f4c63865461785da.jpg?imageView/{mode}/w/{width}/h/{height}",
          "title": "《老炮儿》造型大吐槽！吴亦凡李易峰戏里戏外造型PK！",
          "url": "http://guang.m.yohobuy.com/info/index?id=29630&openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"29630\"},\"update_flag\":\"e9427cb0bf596679b47663f9e8882d99\",\"shareparam\":{\"id\":29630},\"share\":\"/guang/api/v1/share/guang\",\"id\":29630,\"type\":1,\"url\":\"http://guang.m.yohobuy.com/info/index\",\"islogin\":\"N\"}}",
          "views_num": "57424"
        },
        {
          "ads_img_size": "",
          "article_type": "1",
          "author": {
            "author_id": "7304719",
            "avatar": "http://img12.static.yhbimg.com/yhb-img02/2015/06/12/11/02ad0fa0a6066ede986b969840c620540f.jpg?imageView/0/w/100/h/100",
            "name": "胡子小鸡",
            "url": "http://guang.m.yohobuy.com/author/index?id=7304719&openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"7304719\"},\"share\":\"\",\"id\":7304719,\"type\":0,\"islogin\":\"N\",\"url\":\"http://guang.m.yohobuy.com/author/index\"}}"
          },
          "author_id": "7304719",
          "category_id": "2",
          "category_name": "搭配",
          "conver_image_type": "1",
          "id": 24450,
          "intro": "时髦并非一定要绞尽脑汁的叠加搭配。简单的一身装扮也能随性又有魅力！",
          "isFavor": "N",
          "isPraise": "N",
          "is_recommended": "0",
          "praiseStatus": "true",
          "praise_num": "7",
          "publish_time": "11月18日 13:00",
          "share": {
            "url": "http://guang.m.yohobuy.com/info/index?id=24450&openby:yohobuy={\"action\":\"go.share\",\"params\":{\"pic\":\"http://img12.static.yhbimg.com/yhb-img01/2015/11/17/03/024e92ed758690ff392797d46237e3d0f9.jpg?imageView/2/w/640/h/640\",\"title\":\"随便穿穿都很美？那么别错过这一套！\",\"url\":\"http://guang.m.yohobuy.com/info/index?id=24450\",\"content\":\"潮流资讯，新鲜贩售，YOHO!有货【逛】不停\"}}"
          },
          "src": "http://img12.static.yhbimg.com/yhb-img01/2015/11/17/03/024e92ed758690ff392797d46237e3d0f9.jpg?imageView/{mode}/w/{width}/h/{height}",
          "title": "随便穿穿都很美？那么别错过这一套！",
          "url": "http://guang.m.yohobuy.com/info/index?id=24450&openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"24450\"},\"update_flag\":\"286a367f4e35aee5086f340e55cf861e\",\"shareparam\":{\"id\":24450},\"share\":\"/guang/api/v1/share/guang\",\"id\":24450,\"type\":1,\"url\":\"http://guang.m.yohobuy.com/info/index\",\"islogin\":\"N\"}}",
          "views_num": "20812"
        },
        {
          "ads_img_size": "",
          "article_type": "1",
          "author_id": "8168229",
          "category_id": "4",
          "category_name": "潮品",
          "conver_image_type": "2",
          "id": 24454,
          "intro": "自Pump科技诞生以来，Reebok Pump系列下推出了众多受人追捧的经典鞋款。2015年秋冬，Reebok携全新Insta Pump Fury以及 Pump Omni Lite乘风而来，采用低调奢华的设计风格，为众多热爱街头时尚风格的女性，带来女子专属的全新Pump Premium Pack系列。",
          "isFavor": "N",
          "isPraise": "N",
          "is_recommended": "0",
          "praiseStatus": "true",
          "praise_num": "6",
          "publish_time": "11月18日 09:00",
          "share": {
            "url": "http://guang.m.yohobuy.com/info/index?id=24454&openby:yohobuy={\"action\":\"go.share\",\"params\":{\"pic\":\"http://img11.static.yhbimg.com/yhb-img01/2015/11/17/03/01189ad58c2e20e92ec432371d374ed3dc.jpg?imageView/2/w/640/h/640\",\"title\":\"专为“她”属的奢华街头时尚，Reebok 女子Pump Premium Pack系列登陆YOHO!BUY有货\",\"url\":\"http://guang.m.yohobuy.com/info/index?id=24454\",\"content\":\"潮流资讯，新鲜贩售，YOHO!有货【逛】不停\"}}"
          },
          "src": "http://img11.static.yhbimg.com/yhb-img01/2015/11/17/03/01189ad58c2e20e92ec432371d374ed3dc.jpg?imageView/{mode}/w/{width}/h/{height}",
          "title": "专为“她”属的奢华街头时尚，Reebok 女子Pump Premium Pack系列登陆YOHO!BUY有货",
          "url": "http://guang.m.yohobuy.com/info/index?id=24454&openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"24454\"},\"update_flag\":\"b741384cba79577fc882dc4796840c39\",\"shareparam\":{\"id\":24454},\"share\":\"/guang/api/v1/share/guang\",\"id\":24454,\"type\":1,\"url\":\"http://guang.m.yohobuy.com/info/index\",\"islogin\":\"N\"}}",
          "views_num": "12967"
        },
        {
          "ads_img_size": "",
          "article_type": "1",
          "author": {
            "author_id": "3997053",
            "avatar": "http://img13.static.yhbimg.com/yhb-img02/2015/06/12/13/02e09398fe2b9335212f798495f8a8ae6c.jpg?imageView/0/w/100/h/100",
            "name": "艾迪虎",
            "url": "http://guang.m.yohobuy.com/author/index?id=3997053&openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"3997053\"},\"share\":\"\",\"id\":3997053,\"type\":0,\"islogin\":\"N\",\"url\":\"http://guang.m.yohobuy.com/author/index\"}}"
          },
          "author_id": "3997053",
          "category_id": "3",
          "category_name": "潮人",
          "conver_image_type": "1",
          "id": 13577,
          "intro": "此番依旧是走着经典的港式穿搭路线，灰白色的插肩T恤搭配灰色休闲裤，简洁利索的工装元素而又不失街头之感，足下的converse Taylor All Star也是极具街头气息。的确是志明范儿十足。",
          "isFavor": "N",
          "isPraise": "N",
          "is_recommended": "0",
          "praiseStatus": "true",
          "praise_num": "370",
          "publish_time": "10月06日 17:00",
          "share": {
            "url": "http://guang.m.yohobuy.com/info/index?id=13577&openby:yohobuy={\"action\":\"go.share\",\"params\":{\"pic\":\"http://img12.static.yhbimg.com/yhb-img01/2015/09/06/10/026a40897d0dc883ff833634fafa8210d9.jpg?imageView/2/w/640/h/640\",\"title\":\"“志明风”再现，余文乐初秋港式型搭LOOK\",\"url\":\"http://guang.m.yohobuy.com/info/index?id=13577\",\"content\":\"潮流资讯，新鲜贩售，YOHO!有货【逛】不停\"}}"
          },
          "src": "http://img12.static.yhbimg.com/yhb-img01/2015/09/06/10/026a40897d0dc883ff833634fafa8210d9.jpg?imageView/{mode}/w/{width}/h/{height}",
          "title": "“志明风”再现，余文乐初秋港式型搭LOOK",
          "url": "http://guang.m.yohobuy.com/info/index?id=13577&openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"13577\"},\"update_flag\":\"9bed6033d6955da96d5f367260582f4b\",\"shareparam\":{\"id\":13577},\"share\":\"/guang/api/v1/share/guang\",\"id\":13577,\"type\":1,\"url\":\"http://guang.m.yohobuy.com/info/index\",\"islogin\":\"N\"}}",
          "views_num": "94249"
        },
        {
          "ads_img_size": "",
          "article_type": "1",
          "author": {
            "author_id": "3062326",
            "avatar": "http://img12.static.yhbimg.com/yhb-img02/2015/06/12/11/02c5b952480d8c2df55639cd0e918bfd34.jpg?imageView/0/w/100/h/100",
            "name": "V.Z.",
            "url": "http://guang.m.yohobuy.com/author/index?id=3062326&openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"3062326\"},\"share\":\"\",\"id\":3062326,\"type\":0,\"islogin\":\"N\",\"url\":\"http://guang.m.yohobuy.com/author/index\"}}"
          },
          "author_id": "3062326",
          "category_id": "3",
          "category_name": "潮人",
          "conver_image_type": "1",
          "id": 13635,
          "intro": "九月的伦敦温度通常只有十几度，甚至更低，的确有些“动人”~而作为时尚达人的韩火火当然不会被这突如其来的低温所折服。",
          "isFavor": "N",
          "isPraise": "N",
          "is_recommended": "0",
          "praiseStatus": "true",
          "praise_num": "287",
          "publish_time": "10月04日 22:30",
          "share": {
            "url": "http://guang.m.yohobuy.com/info/index?id=13635&openby:yohobuy={\"action\":\"go.share\",\"params\":{\"pic\":\"http://img12.static.yhbimg.com/yhb-img01/2015/09/07/02/023b64bad0c38834392c179942d777e582.jpg?imageView/2/w/640/h/640\",\"title\":\"在冻死人的伦敦，韩火火如何穿的风度与温度并存？\",\"url\":\"http://guang.m.yohobuy.com/info/index?id=13635\",\"content\":\"潮流资讯，新鲜贩售，YOHO!有货【逛】不停\"}}"
          },
          "src": "http://img12.static.yhbimg.com/yhb-img01/2015/09/07/02/023b64bad0c38834392c179942d777e582.jpg?imageView/{mode}/w/{width}/h/{height}",
          "title": "在冻死人的伦敦，韩火火如何穿的风度与温度并存？",
          "url": "http://guang.m.yohobuy.com/info/index?id=13635&openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"13635\"},\"update_flag\":\"9bed6033d6955da96d5f367260582f4b\",\"shareparam\":{\"id\":13635},\"share\":\"/guang/api/v1/share/guang\",\"id\":13635,\"type\":1,\"url\":\"http://guang.m.yohobuy.com/info/index\",\"islogin\":\"N\"}}",
          "views_num": "113109"
        },
        {
          "ads_img_size": "",
          "article_type": "1",
          "author": {
            "author_id": "8123499",
            "avatar": "http://img13.static.yhbimg.com/yhb-img02/2015/06/12/10/0256020c504fb08a176c5457599bdf5b49.jpg?imageView/0/w/100/h/100",
            "name": "Kishi",
            "url": "http://guang.m.yohobuy.com/author/index?id=8123499&openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"8123499\"},\"share\":\"\",\"id\":8123499,\"type\":0,\"islogin\":\"N\",\"url\":\"http://guang.m.yohobuy.com/author/index\"}}"
          },
          "author_id": "8123499",
          "category_id": "4",
          "category_name": "潮品",
          "conver_image_type": "2",
          "id": 13809,
          "intro": "夏天眼看越过越远，空气中渐浓的秋意是不是也提醒着你是时候为衣柜里添加一件卫衣了？",
          "isFavor": "N",
          "isPraise": "N",
          "is_recommended": "0",
          "praiseStatus": "true",
          "praise_num": "26",
          "publish_time": "09月10日 11:10",
          "share": {
            "url": "http://guang.m.yohobuy.com/info/index?id=13809&openby:yohobuy={\"action\":\"go.share\",\"params\":{\"pic\":\"http://img11.static.yhbimg.com/yhb-img01/2015/09/07/12/01b52206e84c37e731636156a397b2c6bd.jpg?imageView/2/w/640/h/640\",\"title\":\"搞怪个性至上！LIGHTNING BEAR满印套头卫衣\",\"url\":\"http://guang.m.yohobuy.com/info/index?id=13809\",\"content\":\"潮流资讯，新鲜贩售，YOHO!有货【逛】不停\"}}"
          },
          "src": "http://img11.static.yhbimg.com/yhb-img01/2015/09/07/12/01b52206e84c37e731636156a397b2c6bd.jpg?imageView/{mode}/w/{width}/h/{height}",
          "title": "搞怪个性至上！LIGHTNING BEAR满印套头卫衣",
          "url": "http://guang.m.yohobuy.com/info/index?id=13809&openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"13809\"},\"update_flag\":\"645414c46742ef3bcf3edc79de153209\",\"shareparam\":{\"id\":13809},\"share\":\"/guang/api/v1/share/guang\",\"id\":13809,\"type\":1,\"url\":\"http://guang.m.yohobuy.com/info/index\",\"islogin\":\"N\"}}",
          "views_num": "10584"
        },
        {
          "ads_img_size": "",
          "article_type": "1",
          "author_id": "8168235",
          "category_id": "2",
          "category_name": "搭配",
          "conver_image_type": "1",
          "id": 13803,
          "intro": "穿的暖暖的装扮除了会让自己更愉悦外，还会让自己看起来更具亲和，在normcore大行其道的当下，暖色调也变成了一种抢手搭配，而驼色的针织衫正是可以让你看起来带些舒适感的单品推荐，配上一条休闲的西裤，加上一双帆布鞋，精致的配饰下看起来简约和休闲，最后喷上一点点香氛让除了视觉上的温暖以外还有一股清香的嗅觉体验。",
          "isFavor": "N",
          "isPraise": "N",
          "is_recommended": "0",
          "praiseStatus": "true",
          "praise_num": "66",
          "publish_time": "09月10日 10:30",
          "share": {
            "url": "http://guang.m.yohobuy.com/info/index?id=13803&openby:yohobuy={\"action\":\"go.share\",\"params\":{\"pic\":\"http://img11.static.yhbimg.com/yhb-img01/2015/09/07/11/016dfccb2cd4f3e9d3ac133d084ec48cef.jpg?imageView/2/w/640/h/640\",\"title\":\"简约舒适normcore，你可能需要来一件驼色针织衫\",\"url\":\"http://guang.m.yohobuy.com/info/index?id=13803\",\"content\":\"潮流资讯，新鲜贩售，YOHO!有货【逛】不停\"}}"
          },
          "src": "http://img11.static.yhbimg.com/yhb-img01/2015/09/07/11/016dfccb2cd4f3e9d3ac133d084ec48cef.jpg?imageView/{mode}/w/{width}/h/{height}",
          "title": "简约舒适normcore，你可能需要来一件驼色针织衫",
          "url": "http://guang.m.yohobuy.com/info/index?id=13803&openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"13803\"},\"update_flag\":\"5b9724501839dfbe5ffcc00201868dc0\",\"shareparam\":{\"id\":13803},\"share\":\"/guang/api/v1/share/guang\",\"id\":13803,\"type\":1,\"url\":\"http://guang.m.yohobuy.com/info/index\",\"islogin\":\"N\"}}",
          "views_num": "39553"
        }
      ]
    },
    "page": 1,
    "total": 38,
    "totalPage": 4
    },
    "md5": "081c03147e3eae753c267a043c7e9b41",
    "message": "星潮教室文章列表"
    }