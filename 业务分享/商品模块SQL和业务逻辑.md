>#product项目#
>
>文档中只涉及数据库的相关查询逻辑，并未包含缓存的相关内容，具体请查询业务代码。
>
>##CommentController 评论##
>###查询商品的最新评论
>1、**yh\_comment** <br>
> `select * from comments where element_id=? and comment_type=1 and status=1 order by create_time desc limit 1`
>
>###分页查询商品评论 
>1、**yh\_comment** <br>
>`select *  from comments  where element_id=？ comment_type=1 and status=1 order by create_time desc limit ?,?` <br>
>2、**yoho\_passport**  查询用户昵称<br>
>`select * from user_base where uid = #{uid} limit 1`
>
>
>##ConsultController 咨询
>###查询咨询总数
>1、**yh_passport**
>
>`select count(1) from consult where product_id =? and answer_user_id > 0`
>
>###查询商品最新咨询
>1、**yh_passport**<br>
>` select * from consult where product_id =? and answer_user_id > 0 order by id DESC limit 1`
>
>###分页查询咨询
>1、**yh_passport**<br>
>`select * from consult where product_id =? and answer_user_id > 0 order by id DESC limit ?,?`
>
>###查询是咨询列表否点赞喜欢、有用
>1、数据存储在redis中
><table>
>    <tr>
>        <td>"yh:like:id:" + consultId</td>
>		<td>咨询被喜欢次数</td>
>    </tr>
> 	<tr>
>        <td>"yh:useful:id:" + consultId</td>
>		<td>咨询被用户点有用次数</td>
>    </tr>
>	<tr>
>        <td>"yh:like:id:" + consultId+":"+uid</td>
>        <td>该用户是否喜该咨询</td>
>    </tr>
>	<tr>
>        <td>"yh:useful:id:" + consultId+":"+uid</td>
>     	<td>该用户是否对该咨询点有用</td>
>    </tr>
></table>
>
>###新增咨询
>1、**yh\_shops** 查询product是否存在根据商品id。<br>
>`select * from product where id=?`
>不存在直接抛异常，中断执行。
>
>2、**yh\_passport** 插入咨询表 <br>
> `insert into consult values()`
>
>###咨询点赞接口
>1、判断是否已经点赞 <br>
> 从redis中取："yh:like:id:" + consultId+":"+uid 判断用户是否已经点赞，已经点赞直接返回。<br>
>2、增加redis中key为："yh:like:id:" + consultId。 咨询的点赞的总数。<br>
>3、增加redis中key为："yh:like:id:" + consultId+":"+uid的值为1，标示已点赞<br>
>4、**yh\_consult**  异步记录点赞表<br>
>  a:`select count(1) from consult_praise_$(consultId%100) where  where consult_id=? and uid=?`   判断是否已经点赞，已经点赞，直接返回 <br>
>  b:`insert into consult_praise_$(consultId%100) (consult_id,uid,create_time) values(?,?,?)`
>
>
>###咨询有用接口
>1、判断用户是否已经点击有用<br>
> 从redis中取："yh:useful:id:" + consultId+":"+uid 判断用户是否已经点击有用，已经点击则直接返回。<br>
>2、增加redis中key为："yh:useful:id:" + consultId。咨询的有用总数。<br>
>3、增加redis中key为："yh:useful:id:" + consultId+":"+uid;的值为1，标示该用户已经点击有用。<br>
>4、**yh\_consult** 异步记录点有用<br>
>  a:`select count(1) from consult_useful_$(consultId%100) where consult_id=? and uid=?`  判断用户是否已经点击有用，已经点有用，直接返回<br>
>  b:` insert into consult_useful_$(consultId%100) (consult_id,uid,create_time) values(?,?,?)` 插入用户点击有用表。
>
>##CouponsController 
>###根据品牌Id查询品牌及优惠券相关信息
>1、缓存中查询品牌相关信息，品牌不存在直接返回。
>2、**yhb\_promotion** <br>
>	`select coupons_id  from brand_coupons where status=2 and brand_id =? order by update_time desc,create_time desc`
>3、**yhb\_promotion** <br>
> ` select * from coupons where id in ()`
>
>##KeywordsController 
>###搜索关键词及平台记录接口
>1、异步插入表 `yh_shops.search_keywords` 表,平台编号
>  <table>
>	<tr><td>pc</td><td>1</td></tr>
>    <tr><td>mobile</td><td>2</td></tr>
>	<tr><td>h5</td><td>3</td></tr>
>	<tr><td>ipad</td><td>4</td></tr>
>  </table>

>
>
>##LimitProductController 限量商品Controller ##
>###查询已经发布的限量商品 ###
>1. 查询限量销售商品
>`select * from yh_shops.limit_product where 1=1 and status=1 and sale_time < UNIX_TIMESTAMP()order by sale_time DESC`
>2. 查询限量销售商品关联商品
>`select * from yh_shops.limit_product_attach where product_id in()`
>3. 封装关联关系并缓存
>4. 根据商品skn（erp_product_id)查询商品ID
>	`select * from yh_shops.product where erp_product_id in()` <br>
>5. 根据商品id查询商品价格
> `select * from yh_shops.product_price where product_id in()`
>6. 封装并返回结果
>
>### 查询热门发售的限量商品 ###
>1. 查询限量热门商品
>  `  select * from yh_shops.limit_product where 1=1 and hotFlag=1 and status=1 order by order_by DESC`
>	（备注：区别于已经发布的是限量商品查询条件)
>2. 查询限量销售商品关联商品
>	`select * from yh_shops.limit_product_attach where product_id in()`
>（备注：limit_product_attach表中的product_id实际是limit_product表中的id,而不是商品表中的Id)
>3. 封装限量销售商品和附件的关联关系并缓存。
>4. 根据商品skn（erp_product_id)查询商品ID
>	`select * from yh_shops.product where erp_product_id in()` <br>
>5. 根据商品id查询商品价格
> `select * from yh_shops.product_price where product_id in()`
>
>###查询即将发售的限量商品###
>1. 查询即将发售的限量商品
> `select * from yh_shops.limit_product where 1=1 and status=1 and showFlag=1 and sale_time > UNIX_TIMESTAMP() order by order_by DESC,sale_time ASC;`
>2. 查询限量销售商品关联商品
>	`select * from yh_shops.limit_product_attach where product_id in()`
>（备注：limit_product_attach表中的product_id实际是limit_product表中的id,而不是商品表中的Id)
>3. 封装限量销售商品和附件的关联关系并缓存。
>4. 根据商品skn（erp_product_id)查询商品ID
>	`select * from yh_shops.product where erp_product_id in()` <br>
>5. 根据商品id查询商品价格
> `select * from yh_shops.product_price where product_id in()`
>
>###根据限量商品code获取限量商品详情###
>1. 根据商品code查询限量商品
> `select * from yh_shops.limit_product  where code =?  and status=1`
>
>2. 查询限量销售商品关联商品
>	`select * from yh_shops.limit_product_attach where product_id in()`
>（备注：limit_product_attach表中的product_id实际是limit_product表中的id,而不是商品表中的Id)
>
>3. 封装限量销售商品和附件的关联关系并缓存。
>4. 根据商品skn（erp_product_id)查询商品ID
>	`select * from yh_shops.product where erp_product_id in()` <br>
>5. 根据商品id查询商品价格
> `select * from yh_shops.product_price where product_id in()`
>
>###根据活动ID查询限量商品###
>1.根据活动ID查询限量商品
>`select * form yh_shops.limit_product where activityId=? and status=1`
>2. 查询限量销售商品关联商品
>	`select * from yh_shops.limit_product_attach where product_id in()`
>（备注：limit_product_attach表中的product_id实际是limit_product表中的id,而不是商品表中的Id)
>
>
>###批量根据限量商品code获取限量商品详情###
>1. 根据商品code列表查询商品详情
>
> select * from yh_shops.limit_product where code in ()
> 
>2. 查询限量商品关联的商品
> `select * from yh_shops.limit_product_attach where product_id in()`
>3. 封装限量销售商品和附件的关联关系并缓存。
>4. 根据商品skn（erp_product_id)查询商品ID
>	`select * from yh_shops.product where erp_product_id in()` <br>
>5. 根据商品id查询商品价格
> `select * from yh_shops.product_price where product_id in()`
>###已经发售的商品总数###
>1. 查询已经发售的商品总数
>`select count(1) from yh_shops.limit_product where sale_time<UNIX_TIMESTAMP() and status=1`
>
>
>###热门发售商品的发售总数###
>1. 查询热门发售商品的发售总数
> `select count(1) from yh_shops.limit_product where hotFlag=1 and status=1`
>
>###即将发售的商品总数##
>1.查询即将发售的商品总数
>> `select count(1) from yh_shops.limit_product where sale_time>UNIX_TIMESTAMP() and status=1 and showFlag=1`
>
>###给后台提供的新增限量商品###
> 
> 1. 校验限量商品是否存在  
> `select * from yh_shops.limit_product where code=?`  
> 2. 插入限量商品  
> `insert into yh_shops.limit_product values();`  
> 3. 
> 















#promotion模块

##CouponController
###发送优惠券 flag=1支持重复发送###
1. 检查用户是否已经领取过优惠券
> select count(1) from yhb_promotion.coupons_logs where uid=8038725 and coupon_id=11759;

2. 检查券是否存在
> select * from yhb_promotion.coupons where id= 11759;

3.  查询券类型是否存在

> select * from yhb_promotion.coupon_type ;

4. 判断yhb_promotion.coupons.custom_type 是否存在，存在需要校验用户的会员级别等信息

5. 查询一个可用的优惠券
> select * from yhb_promotion.coupons_sn where is_use="N" limit 1;
> select * from yhb_promotion.a_coupons_sn where is_use="N" limit 1;
> select * from yhb_promotion.b_coupons_sn where is_use="N" limit 1;

6. 增加券的领用记录(包括用户id，是否重复领用标记，券号等）

> insert into yhb_promotion.coupons_logs value();
7. 缓存中增加券的领用数量，并设置用户的领券状态

8. 记录用户领券同步表（用户id，券号等）

>insert into  yhb_promotion.user_coupon_logs_sync value();




