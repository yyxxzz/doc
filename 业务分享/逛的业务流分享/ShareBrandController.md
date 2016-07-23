# 一、getShareBrandInfo()方法：获取分享品牌详情服务 #

**请求参数**

| 参数名称 | 参数类型 | 可否为空 |示例  |默认值 |备注  |
| ---------|:--------:| --------:|-----:|------:|-----:|
|id|int    | 否       |21     |0      |id|


**实现逻辑：**

1、ShareBrandController.getShareBrandInfo()方法中调用sns.getShareBrandInfo服务；

2、对参数id进行校验，若值小于1，则抛出分享品牌详情id为空异常；

3、根据id查询数据库表brand_relation，获取该id对应品牌相关信息，BrandRelation对象，从该BrandRelation对象中获取对应的品牌id值brand\_id;

4、通过该brand\_id作为参数调用product接口获取该品牌的详细产品信息，得到BrandBo对象.

5、组合查询到的品牌信息与品牌产品详细信息，返回封装的ShareBrandRspBO对象.

**示例：**
请求：
      http://localhost:8080/gateway/guang/api/*/share/brandinfo?id=5&debug=XYZ


返回：
     
     {
     "alg": "SALT_MD5",
     "code": 200,
     "data": {
     "content": "澳洲鞋履品牌WINDSOR SMITH , 自1946年创立于澳洲墨尔本, 从男裝鞋履起家,\n及至2008年在墨尔本潮流集中地Chapel Street开设首间专门店\n\n到2009年, WINDSOR SMITH首間女裝鞋履专门店也正式开业。 在短短数年間,\n已经成为澳洲热手可炙，世界各地年轻人热烈追棒的No.1年轻潮流品牌, 更在澳洲鞋履品牌Instagram中, 拥有最多支持者的牌子。\n\nWINDSOR SMITH現由企业的第4代家族成员主理,\n设计团队年轻具活力和潮流前瞻性触觉，当中一位家族成员更是澳洲其中一位最年轻的鞋履设计总监。\n\n品牌主張简洁而极具街头个性的年轻风格, 款式注目度甚高, 用心的细节位令设计更具立体感。采用外国进口高质量的皮革材料,\n舒适却不失个人风格, 创造出前卫而不失灵活度的潮流鞋履。",
     "pic": "http://img10.static.yhbimg.com/brandLogo/2015/12/03/16/013fe1e1361382ae8e7d9a545863e098c2.png?imageMogr2/thumbnail/166x166/extent/166x166/background/d2hpdGU=/position/center/quality/80",
     "title": "WINDSOR SMITH",
     "url": "http://guang.m.yohobuy.com/plustar/brandinfo?id=21"
      },
     "md5": "07f609b2ba24eda27125aa1e7932e9ae",
     "message": "品牌详情分享"
     }
     
     
# 二、getPlustarList()方法：获取明星原创品牌列表#

**请求参数**

| 参数名称 | 参数类型 | 可否为空 | 示例 | 默认值 | 备注 |
| -----|:----:| ----:| ----:| ----:| ----:|
| brand_type    | byte    | 否   |1   |-   |品牌类别  |
| gender    | string    |  否   | 1   |-   |性别  |

**实现逻辑：**

1、ShareBrandController.getPlustarList()方法中调用sns.getPlustarList服务；

2、对参数brand\_type进行校验，若值小于1，则抛出品牌id为空异常；

3、根据brand_type查询数据库表plustar\_category，校验分类列表是否包含该brand\_Type，若不包含，则抛出品牌为空异常，若包含，则获取PlustarCategory对象，该对象包括品牌名、品牌编号等信息；

4、根据brand\_Type, gender参数查询数据库表brand\_relation，获取该品牌关系列表；

5、根据品牌关系列表id查询数据库表brand\_img,获取品牌图片列表；

6、拼装品牌信息、品牌关系列表与品牌图片列表信息，构造返回对象PlustarRespBO.

**示例：**
请求：
     http://localhost:8080/gateway/guang/api/*/plustar/getlist?brand_type=1&gender=1&debug=XYZ


返回：
     
     {
    "alg": "SALT_MD5",
    "code": 200,
    "data": {
        "brand_type": 1,
        "brand_type_name": "潮流经典",
        "data": {
            "foot": [],
            "head": [],
            "list": [
                {
                    "data": [
                        {
                            "brand_id": "30",
                            "brand_name": "Chetti Rouge轩谛",
                            "brand_title": "达到的说法的广泛丰厚的",
                            "data": [
                                {
                                    "src": "http://img12.static.yhbimg.com/yhb-img01/2016/03/28/08/02a4da74a108380e93a33dc5eb91b42f2f.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"438\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":438},\"id\":438,\"type\":0,\"islogin\":\"N\"},\"id\":438,\"title\":\"Chetti Rouge轩谛\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "438",
                            "sort_id": "506"
                        },
                        {
                            "brand_id": "361",
                            "brand_name": "Stussy",
                            "brand_title": "Stussy国际街头先锋，融入滑板服、旧校服等元素，掀起独特的街头潮流。",
                            "data": [
                                {
                                    "src": "http://img11.static.yhbimg.com/yhb-img01/2015/07/24/14/010c0af0e91e6e0a8a85de35e14a127cd4.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"103\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":103},\"id\":103,\"type\":0,\"islogin\":\"N\"},\"id\":103,\"title\":\"Stussy\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "103",
                            "sort_id": "450"
                        },
                        {
                            "brand_id": "439",
                            "brand_name": "Human Made",
                            "brand_title": "由A BATHING APE主理NIGO全新打造的以古着风格设计为主打的品牌。",
                            "data": [
                                {
                                    "src": "http://img11.static.yhbimg.com/yhb-img01/2015/07/14/15/01e55bc1dd0d754cbdd9595614e1491f4d.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"241\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":241},\"id\":241,\"type\":0,\"islogin\":\"N\"},\"id\":241,\"title\":\"Human Made\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "241",
                            "sort_id": "440"
                        },
                        {
                            "brand_id": "429",
                            "brand_name": "Undefeated",
                            "brand_title": "作为资深街头品牌Undefeated努力打破刻板重复、缺乏新意的局面，积极求变。",
                            "data": [
                                {
                                    "src": "http://img11.static.yhbimg.com/yhb-img01/2015/10/16/10/016c6d502b094c3e17762cbd4d24b96a30.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"325\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":325},\"id\":325,\"type\":0,\"islogin\":\"N\"},\"id\":325,\"title\":\"Undefeated\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "325",
                            "sort_id": "430"
                        },
                        {
                            "brand_id": "654",
                            "brand_name": "BOY LONDON",
                            "brand_title": "英伦街牌的鼻祖骨子里透着叛逆不断以颠覆理念挑战传统服饰行业。",
                            "data": [
                                {
                                    "src": "http://img11.static.yhbimg.com/yhb-img01/2015/12/08/05/018ae9a08b77616112d76f3e3356be90fb.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"374\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":374},\"id\":374,\"type\":0,\"islogin\":\"N\"},\"id\":374,\"title\":\"BOY LONDON\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "374",
                            "sort_id": "400"
                        },
                        {
                            "brand_id": "596",
                            "brand_name": "Carhartt WIP",
                            "brand_title": "Carhartt WIP以一种前卫和创新的方式延续着Carhartt的精髓。",
                            "data": [
                                {
                                    "src": "http://img11.static.yhbimg.com/yhb-img01/2015/07/14/15/01a5d739d2b6e43cad97255387cbbf694e.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"235\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":235},\"id\":235,\"type\":0,\"islogin\":\"N\"},\"id\":235,\"title\":\"Carhartt WIP\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "235",
                            "sort_id": "370"
                        },
                        {
                            "brand_id": "354",
                            "brand_name": "CHEAP MONDAY",
                            "brand_title": "CHEAP MONDAY的愿景和使命“把第一条牛仔裤送上月球”。",
                            "data": [
                                {
                                    "src": "http://img12.static.yhbimg.com/yhb-img01/2015/09/10/06/02072bfd787b9fcd28d5e234f8ed0d3780.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"95\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":95},\"id\":95,\"type\":0,\"islogin\":\"N\"},\"id\":95,\"title\":\"CHEAP MONDAY\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "95",
                            "sort_id": "360"
                        },
                        {
                            "brand_id": "832",
                            "brand_name": "CHUMS",
                            "brand_title": "“我希望创立一个亲民且源于生活融入生活的品牌。”",
                            "data": [
                                {
                                    "src": "http://img11.static.yhbimg.com/yhb-img01/2015/08/28/08/01343a3ac0ac83b5381258d2cc4600de93.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"273\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":273},\"id\":273,\"type\":0,\"islogin\":\"N\"},\"id\":273,\"title\":\"CHUMS\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "273",
                            "sort_id": "350"
                        },
                        {
                            "brand_id": "343",
                            "brand_name": "DC",
                            "brand_title": "DC Shoes作为Quiksilver旗下的品牌，已经在板鞋中领跑多年。",
                            "data": [
                                {
                                    "src": "http://img10.static.yhbimg.com/yhb-img01/2015/07/24/13/01d5631fcd315be27b5b0a3a78b19e4721.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"257\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":257},\"id\":257,\"type\":0,\"islogin\":\"N\"},\"id\":257,\"title\":\"DC\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "257",
                            "sort_id": "340"
                        },
                        {
                            "brand_id": "696",
                            "brand_name": "Diamond SUPPLY CO.",
                            "brand_title": "以TIFFANY钻石作为品牌主旨的美国潮牌【DIAMOND SUPPLY CO.】",
                            "data": [
                                {
                                    "src": "http://img12.static.yhbimg.com/yhb-img01/2015/08/17/07/02b40acad0fa5618641f81cbe02c930378.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"129\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":129},\"id\":129,\"type\":0,\"islogin\":\"N\"},\"id\":129,\"title\":\"Diamond SUPPLY CO.\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "129",
                            "sort_id": "330"
                        },
                        {
                            "brand_id": "577",
                            "brand_name": "DOPE",
                            "brand_title": "美式品牌DOPE为我们展现独有的气质 要有自己特征。",
                            "data": [
                                {
                                    "src": "http://img11.static.yhbimg.com/yhb-img01/2015/06/02/18/01345037cb8a6d542506e8be132acba1ec.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"109\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":109},\"id\":109,\"type\":0,\"islogin\":\"N\"},\"id\":109,\"title\":\"DOPE\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "109",
                            "sort_id": "320"
                        },
                        {
                            "brand_id": "559",
                            "brand_name": "DIMEPIECE",
                            "brand_title": "不羁的图形，特立独行。蕾哈娜最爱！",
                            "data": [
                                {
                                    "src": "http://img12.static.yhbimg.com/yhb-img01/2015/10/15/09/029e4eb5d517bcce21355640af812b8f39.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"119\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":119},\"id\":119,\"type\":0,\"islogin\":\"N\"},\"id\":119,\"title\":\"DIMEPIECE\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "119",
                            "sort_id": "310"
                        },
                        {
                            "brand_id": "699",
                            "brand_name": "DGK",
                            "brand_title": "美国街头滑板品牌。",
                            "data": [
                                {
                                    "src": "http://img12.static.yhbimg.com/yhb-img01/2015/12/08/09/025d5cb77d7b30b9c3bde4b8546491458c.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"418\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":418},\"id\":418,\"type\":0,\"islogin\":\"N\"},\"id\":418,\"title\":\"DGK\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "418",
                            "sort_id": "305"
                        },
                        {
                            "brand_id": "631",
                            "brand_name": "ELEVEN PARIS",
                            "brand_title": "法国高街品牌，其中LIAJ设计线每季都带来跨界合作的新鲜视觉",
                            "data": [
                                {
                                    "src": "http://img11.static.yhbimg.com/yhb-img01/2015/06/02/18/01967d5b1d9801035588f34347cb372d26.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"125\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":125},\"id\":125,\"type\":0,\"islogin\":\"N\"},\"id\":125,\"title\":\"ELEVEN PARIS\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "125",
                            "sort_id": "300"
                        },
                        {
                            "brand_id": "50",
                            "brand_name": "G-SHOCK",
                            "brand_title": "创造一块永不摔坏的手表。",
                            "data": [
                                {
                                    "src": "http://img11.static.yhbimg.com/yhb-img01/2015/12/16/03/01551bb415d62780fbe6d1669bf38bed96.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"422\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":422},\"id\":422,\"type\":0,\"islogin\":\"N\"},\"id\":422,\"title\":\"G-SHOCK\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "422",
                            "sort_id": "295"
                        },
                        {
                            "brand_id": "690",
                            "brand_name": "HALFMAN",
                            "brand_title": "品牌HALFMAN设计团队以及设计总监皆来自洛杉矶",
                            "data": [
                                {
                                    "src": "http://img10.static.yhbimg.com/yhb-img01/2015/05/21/21/0161da9e215df4c34333acfc09ecd52035.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"31\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":31},\"id\":31,\"type\":0,\"islogin\":\"N\"},\"id\":31,\"title\":\"HALFMAN\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "31",
                            "sort_id": "280"
                        },
                        {
                            "brand_id": "603",
                            "brand_name": "HALL OF FAME",
                            "brand_title": "加州人气街头潮牌，因其现代化的设计在嘻哈圈有著高居不下的人气",
                            "data": [
                                {
                                    "src": "http://img10.static.yhbimg.com/yhb-img01/2015/06/26/13/015b21f850a724ad7c50bc6d43fc09dbef.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"37\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":37},\"id\":37,\"type\":0,\"islogin\":\"N\"},\"id\":37,\"title\":\"HALL OF FAME\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "37",
                            "sort_id": "270"
                        },
                        {
                            "brand_id": "410",
                            "brand_name": "Head Porter",
                            "brand_title": "Porter袋早在1935年由吉田吉藏先生在东京而有名 。",
                            "data": [
                                {
                                    "src": "http://img11.static.yhbimg.com/yhb-img01/2015/06/02/19/014b827a75f03487641e753bf89e56d5e2.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"127\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":127},\"id\":127,\"type\":0,\"islogin\":\"N\"},\"id\":127,\"title\":\"Head Porter\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "127",
                            "sort_id": "260"
                        },
                        {
                            "brand_id": "606",
                            "brand_name": "HLZ BLZ",
                            "brand_title": "纽约主打女装嘻哈风格,自然不做作且充满爆发力的女孩们。",
                            "data": [
                                {
                                    "src": "http://img10.static.yhbimg.com/yhb-img01/2015/05/21/21/013d8a21c4170216de643dfb090af41d3f.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"39\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":39},\"id\":39,\"type\":0,\"islogin\":\"N\"},\"id\":39,\"title\":\"HLZ BLZ\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "39",
                            "sort_id": "250"
                        },
                        {
                            "brand_id": "578",
                            "brand_name": "HUF",
                            "brand_title": "旧金山知名街头潮流品牌HUF.",
                            "data": [
                                {
                                    "src": "http://img11.static.yhbimg.com/yhb-img01/2015/10/16/10/0159034ced974e57102a349ee61641e9f5.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"53\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":53},\"id\":53,\"type\":0,\"islogin\":\"N\"},\"id\":53,\"title\":\"HUF\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "53",
                            "sort_id": "240"
                        },
                        {
                            "brand_id": "712",
                            "brand_name": "hype",
                            "brand_title": "“想办法点燃你自己，创意就像滚雪球会越变越大”——Hype",
                            "data": [
                                {
                                    "src": "http://img11.static.yhbimg.com/yhb-img01/2015/10/16/10/01c86c313e563fe17237377f2873e0db87.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"311\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":311},\"id\":311,\"type\":0,\"islogin\":\"N\"},\"id\":311,\"title\":\"hype\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "311",
                            "sort_id": "230"
                        },
                        {
                            "brand_id": "950",
                            "brand_name": "Losers",
                            "brand_title": "来自日本原宿的鞋履品牌LOSERS，细腻简约。",
                            "data": [
                                {
                                    "src": "http://img12.static.yhbimg.com/yhb-img01/2015/10/16/10/024f42ee710ec2c41d87b6b517db5143f4.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"317\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":317},\"id\":317,\"type\":0,\"islogin\":\"N\"},\"id\":317,\"title\":\"Losers\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "317",
                            "sort_id": "220"
                        },
                        {
                            "brand_id": "438",
                            "brand_name": "Medicom Toy",
                            "brand_title": "深受潮流人士喜爱的日本玩具生产商的佼佼者MEDICOM TOY。",
                            "data": [
                                {
                                    "src": "http://img11.static.yhbimg.com/yhb-img01/2015/10/16/10/013390744aa78291a4b52aaa24ec4ca927.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"319\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":319},\"id\":319,\"type\":0,\"islogin\":\"N\"},\"id\":319,\"title\":\"Medicom Toy\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "319",
                            "sort_id": "210"
                        },
                        {
                            "brand_id": "657",
                            "brand_name": "MOUSSY",
                            "brand_title": "日本超人气时尚女装潮牌MOUSSY。",
                            "data": [
                                {
                                    "src": "http://img11.static.yhbimg.com/yhb-img01/2015/11/04/09/01501f091a74bcd99a2782282131f8358d.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"335\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":335},\"id\":335,\"type\":0,\"islogin\":\"N\"},\"id\":335,\"title\":\"MOUSSY\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "335",
                            "sort_id": "190"
                        },
                        {
                            "brand_id": "268",
                            "brand_name": "New Era",
                            "brand_title": "全球领先的头饰设计者、开发商和制造商。",
                            "data": [
                                {
                                    "src": "http://img12.static.yhbimg.com/yhb-img01/2015/12/08/09/02880b8b4d220661aaddfb28d237dae965.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"420\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":420},\"id\":420,\"type\":0,\"islogin\":\"N\"},\"id\":420,\"title\":\"New Era\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "420",
                            "sort_id": "185"
                        },
                        {
                            "brand_id": "632",
                            "brand_name": "Publish",
                            "brand_title": "美国加州街头品牌，以其特色标志性束脚裤（Jogger Pants）而闻名",
                            "data": [
                                {
                                    "src": "http://img11.static.yhbimg.com/yhb-img01/2015/08/17/07/015bd40038a2d5c2b2510b69d74ca8b506.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"35\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":35},\"id\":35,\"type\":0,\"islogin\":\"N\"},\"id\":35,\"title\":\"Publish\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "35",
                            "sort_id": "180"
                        },
                        {
                            "brand_id": "674",
                            "brand_name": "Red Wing",
                            "brand_title": "从当初传统的工作靴到现在的鞋中之王",
                            "data": [
                                {
                                    "src": "http://img10.static.yhbimg.com/yhb-img01/2015/05/21/21/0191bc666f39bde2b506b555ce1bc7ca63.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"49\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":49},\"id\":49,\"type\":0,\"islogin\":\"N\"},\"id\":49,\"title\":\"Red Wing\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "49",
                            "sort_id": "170"
                        },
                        {
                            "brand_id": "778",
                            "brand_name": "SLY",
                            "brand_title": "日本巴罗克服饰集团旗下时尚前沿品牌SLY人气潮牌MOUSSY的姐妹品牌.",
                            "data": [
                                {
                                    "src": "http://img11.static.yhbimg.com/yhb-img01/2015/10/15/09/015e97107e5de81bddf271aca3c14e2453.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"259\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":259},\"id\":259,\"type\":0,\"islogin\":\"N\"},\"id\":259,\"title\":\"SLY\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "259",
                            "sort_id": "160"
                        },
                        {
                            "brand_id": "430",
                            "brand_name": "SSUR",
                            "brand_title": "以恶搞，讽刺各大品牌而闻名的纽约地下街头品牌SSUR。",
                            "data": [
                                {
                                    "src": "http://img11.static.yhbimg.com/yhb-img01/2015/07/09/17/01e9ffa1bf7f1b5b7d110e01054b9a7e65.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"105\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":105},\"id\":105,\"type\":0,\"islogin\":\"N\"},\"id\":105,\"title\":\"SSUR\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "105",
                            "sort_id": "150"
                        },
                        {
                            "brand_id": "648",
                            "brand_name": "STAPLE",
                            "brand_title": "STAPLE以鸽子主题元素著称的纽约潮流品牌，完美的诠释了街头文化和生活方式。",
                            "data": [
                                {
                                    "src": "http://img12.static.yhbimg.com/yhb-img01/2015/10/15/09/02753b4923b902715e62accbc71d0e7657.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"123\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":123},\"id\":123,\"type\":0,\"islogin\":\"N\"},\"id\":123,\"title\":\"STAPLE\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "123",
                            "sort_id": "140"
                        },
                        {
                            "brand_id": "549",
                            "brand_name": "The Hundreds",
                            "brand_title": "The Hundreds根源于LA的80-90年代南加州西海岸风格，深受街头潮人的喜爱.。",
                            "data": [
                                {
                                    "src": "http://img11.static.yhbimg.com/yhb-img01/2015/10/15/09/0108098bd244969d6582154529acfb416e.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"111\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":111},\"id\":111,\"type\":0,\"islogin\":\"N\"},\"id\":111,\"title\":\"The Hundreds\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "111",
                            "sort_id": "130"
                        },
                        {
                            "brand_id": "201",
                            "brand_name": "wesc",
                            "brand_title": "首个将音乐元素引进高级街头时装的品牌,取名自创意、文化、自由",
                            "data": [
                                {
                                    "src": "http://img10.static.yhbimg.com/yhb-img01/2015/05/21/21/01d922c7234bc01905f915ea1d02856186.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"27\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":27},\"id\":27,\"type\":0,\"islogin\":\"N\"},\"id\":27,\"title\":\"wesc\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "27",
                            "sort_id": "120"
                        },
                        {
                            "brand_id": "554",
                            "brand_name": "X-GIRL",
                            "brand_title": "X-GIRL女性分支品牌，属于自信独立，骨子里前卫反叛个性",
                            "data": [
                                {
                                    "src": "http://img12.static.yhbimg.com/yhb-img01/2015/10/15/09/02b9de92e16a0c2b9d68bbddb7492b0f8a.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"121\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":121},\"id\":121,\"type\":0,\"islogin\":\"N\"},\"id\":121,\"title\":\"X-GIRL\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "121",
                            "sort_id": "110"
                        },
                        {
                            "brand_id": "747",
                            "brand_name": "X-LARGE",
                            "brand_title": "已拥有22年历史的潮牌X-LARGE擅长以融合滑板、rave及hip-hop等多种年轻流行文化。",
                            "data": [
                                {
                                    "src": "http://img11.static.yhbimg.com/yhb-img01/2015/10/16/10/01c584ab95bd290ecd42d1394872863561.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"223\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":223},\"id\":223,\"type\":0,\"islogin\":\"N\"},\"id\":223,\"title\":\"X-LARGE\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "223",
                            "sort_id": "100"
                        },
                        {
                            "brand_id": "513",
                            "brand_name": "Us Versus Them",
                            "brand_title": "近年来颇受街头时尚爱好者青睐的美国南加州品牌 Us Versus Them。",
                            "data": [
                                {
                                    "src": "http://img12.static.yhbimg.com/yhb-img01/2015/10/16/10/0210c20ac63c656fce67e3b1a0ea6011d8.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"329\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":329},\"id\":329,\"type\":0,\"islogin\":\"N\"},\"id\":329,\"title\":\"Us Versus Them\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "329",
                            "sort_id": "73"
                        },
                        {
                            "brand_id": "688",
                            "brand_name": "WINDSOR SMITH",
                            "brand_title": "品牌品牌",
                            "data": [],
                            "id": "21",
                            "sort_id": "15"
                        },
                        {
                            "brand_id": "774",
                            "brand_name": "Oni signal",
                            "brand_title": "为追求自我生活态度和独立风格的女性而设计。",
                            "data": [
                                {
                                    "src": "http://img11.static.yhbimg.com/yhb-img01/2015/09/10/06/0135ab657777a60b724eaf260484c997d2.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"281\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":281},\"id\":281,\"type\":0,\"islogin\":\"N\"},\"id\":281,\"title\":\"Oni signal\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "281",
                            "sort_id": "0"
                        },
                        {
                            "brand_id": "241",
                            "brand_name": "Onitsuka Tiger",
                            "brand_title": "Onitsuka Tiger为ASICS的前身，在世界体坛留下无数王者传奇。",
                            "data": [
                                {
                                    "src": "http://img11.static.yhbimg.com/yhb-img01/2015/08/17/07/01d0b04c9235315af95b46a7f4f773bb76.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"267\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":267},\"id\":267,\"type\":0,\"islogin\":\"N\"},\"id\":267,\"title\":\"Onitsuka Tiger\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "267",
                            "sort_id": "0"
                        },
                        {
                            "brand_id": "364",
                            "brand_name": "Levi’s",
                            "brand_title": "Levi's象征着美国野性、刚毅、叛逆与美国开拓者的精神。",
                            "data": [
                                {
                                    "src": "http://img12.static.yhbimg.com/yhb-img01/2015/08/17/07/02a7a536b55afce2439b97d05812406740.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"265\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":265},\"id\":265,\"type\":0,\"islogin\":\"N\"},\"id\":265,\"title\":\"Levi’s\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "265",
                            "sort_id": "0"
                        },
                        {
                            "brand_id": "432",
                            "brand_name": "Married to the MOB",
                            "brand_title": "纯粹纽约街头风格，纯粹纽约态度！",
                            "data": [
                                {
                                    "src": "http://img11.static.yhbimg.com/yhb-img01/2015/05/21/21/012a4d6cecc362ae7557fc8bb6d9d7507d.jpg?imageView/{mode}/w/{width}/h/{height}",
                                    "url": "http://guang.m.yohobuy.com/guang/plustar/brandinfo?openby:yohobuy={\"action\":\"go.h5\",\"params\":{\"param\":{\"id\":\"57\"},\"share\":\"/guang/api/v1/share/brandinfo\",\"shareparam\":{\"param\":{\"gender\":\"1,3\",\"id\":57},\"id\":57,\"type\":0,\"islogin\":\"N\"},\"id\":57,\"title\":\"Married to the MOB\",\"url\":\"http://guang.m.yohobuy.com/guang/plustar/brandinfo\"}}"
                                }
                            ],
                            "id": "57",
                            "sort_id": "0"
                        }
                    ],
                    "template_intro": "品牌列表",
                    "template_name": "brand_list"
                }
            ]
        }
    },
    "md5": "ec6f7325a5de7b65324c6f89133ea64f",
    "message": "品牌列表"
}

# 三、getBrandInfo()方法：根据品牌关系id，获取品牌详情#

**请求参数**

| 参数名称 | 参数类型 | 可否为空 |示例  |默认值 |备注  |
| ---------|:--------:| --------:|-----:|------:|-----:|
|id|string    | 否       |121     |-      |品牌关系id|


**实现逻辑：**

1、ShareBrandController.getBrandInfo()方法中调用sns.getBrandInfo服务；

2、对参数id进行校验，若值小于1，则抛出id为空异常；

3、根据id查询数据库表brand_relation，查询是否存在该id号的品牌关系，若不存在则抛出异常；若存在，则返回品牌id列表；

4、根据品牌id列表调用product.queryBrandByIds()接口查询具体品牌信息；

5、查询数据库表plustar_category，获取Status为1的品牌类型记录；

6、拼装品牌关系列表、品牌信息与品牌类型，构造返回对象PlustarBrandInfoRespBO.

**示例：**
请求：
     http://localhost:8080/gateway/guang/api/*/plustar/getbrandinfo?id=121&debug=XYZ


返回：

    {
     "alg": "SALT_MD5",
     "code": 200,
     "data": {
     "brand_domain": "xgirl",
     "brand_ico": "/2015/12/09/14/01f9101ac1a25cfa5dfc526d0b3c54facf.jpg",
     "brand_id": 554,
     "brand_intro": "X-GIRL自1994年成立，由Sonic Youth乐队主唱兼贝斯手Kim\nGordon一手创建，作为X-Large的女性分支品牌踩入街牌行列，作为日系休闲服装的一大亮点，极具传奇色彩。自信、独立，而且骨子里更带点前卫反叛的个性，令一群惺惺相惜的潮流少女们纷纷“捧场”。X-GIRL一直以来走着甜美少女味，一望即知是日本热情而又随性的小女生典范。款式简约，而色彩和图案则作为点睛之笔存在得举足轻重，广受热爱Casual\nWear的女生欢迎。<p><img border=\"0\" src=\n\"http://img02.static.yohobuy.com/brandContentImg/2014/09/17/10/022e552896b0412451e87fef33fbb02de7.jpg\" /></p>\n<p><img border=\"0\" src=\n\"http://img01.static.yohobuy.com/brandContentImg/2014/09/17/10/01ae874e679ba9561af96bdde0a6dd21f0.jpg\" /></p>\n<p><img border=\"0\" src=\n\"http://img01.static.yohobuy.com/brandContentImg/2014/09/17/10/01c82c7487b13833422fc26870e0107541.jpg\" /></p>\n<p><img border=\"0\" src=\n\"http://img02.static.yohobuy.com/brandContentImg/2014/09/17/10/02862090922c14ad387d8a929cd16ef7ee.jpg\" /></p>\n<p><img border=\"0\" src=\n\"http://img02.static.yohobuy.com/brandContentImg/2014/09/17/10/0235da76a2ffbb1f5323f80a95cc795e13.jpg\" /></p>\n<p><img border=\"0\" src=\n\"http://img02.static.yohobuy.com/brandContentImg/2014/09/17/10/02dce1467d2d2709827d8a8781d0154637.jpg\" /></p>\n<p><img border=\"0\" src=\n\"http://img01.static.yohobuy.com/brandContentImg/2014/09/17/10/01a1c133e35645e38e6eaeb39fda752322.jpg\" /></p>\n<p><img border=\"0\" src=\n\"http://img01.static.yohobuy.com/brandContentImg/2014/09/17/10/015a1b5759872a6f880838e7a5d23754f9.jpg\" /></p>\n",
     "brand_name": "X-GIRL",
     "brand_type": 0,
     "brand_type_name": "潮流经典",
     "cover_img": "http://img12.static.yhbimg.com/yhb-img01/2015/10/15/09/026a8bbe4d45c1b25339fac25a645106fe.jpg?imageView/{mode}/w/{width}/h/{height}",
     "id": 121,
     "is_different": 1,
     "is_recommend": 2,
     "status": 1
     },
     "md5": "6a191dfefb3ec8f230e71d472968b8ba",
     "message": "品牌详情"
    }