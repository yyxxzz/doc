算费上下文ChargeContext构建具体过程
----
### ChargeContext由ChargeContextFactory.build()构建，参数：Boolean selected、ChargeParam，
### ChargeContext需要构建的信息：
	用户信息：vip、当月订单数、有货币、红包
	购物车中商品的详情：价格，是否货到付款，是否下架，outlet、促销信息等
### 具体实现过程：
    根据ChargeParam中的List<ShoppingItem>是否为空类进行不同的构建过程.
    如果为空则调用buildByShoppingCart通过购物车构建。
    如果不为空，则是立即购物，通过buildByShoppingItems进行构建。
### buildByShoppingCart构建过程如下：
	1）通过ChargeParam中的uid和shoppingkey找到购物车对象ShoppingCart，为空则抛异常。
		如果uid>0，通过uid到数据库表shoppingCart查询到物车对象ShoppingCart
		如果uid<=0再通过shoppingkey，到数据库表shoppingCart查询到物车对象ShoppingCart。
	2）根据ShoppingCart设置ChargeParam的属性（uid，shoppingkey，shoppingcarid）
		因为uid或shoppingkey可能有一个为空，所以查询到购物车后统一更新这两个值
	3）查询设置商品的详细信息List<ShoppingCartItems> shoppingCartGoodsList。
		根据shoppingCartId到数据库表shopping_cart_items中
		查询当前购物车中商品的信息List<ShoppingCartItems> goodsList。
		从goodsList获取到skns、skus
		根据skns到商品模块查询到商品的详细信息
		然后将商品的各种信息（价格，是否货到付款，是否下架，outlet等保存到good的extmap中）
		根据skns到促销模块查询商品的购买数量限制信息，并设置good的购买限制，
		根据good的促销id查询促销信息（先到缓存，再到数据库中查询）
		根据促销信息设置good的promotion_type和real_price、last_Price
		promotion_type=Gift为赠品，real和last Price为0，
		promotion_type=Needpaygift为加价购商品，real和last Price为add_cost，
	4）根据ChargeParam和shoppingCartGoodsList调用doBuild()构建ChargeContext并返回
		将shoppingCartGoodsList转换成List<ChargeGoods>（利用反射将map中的属性设置到ChargeGoods中）
		并将List<ChargeGoods>保存到ChargeContext。
		根据uid到用户模块和数据获取各种相关信息，并保持到UserInfo中。
		调用users.getVipSimpleInfo服务，获取用户的vip信息。
		到order表查询用户当月点单数。
		调用users.getYohoCoin服务，获取用户的有货币信息，并转化成货币。
		调用users.selectRedenvelopesCount服务，获取用户的红包，
		将UserInfo保存到ChargeContext中。
### buildByShoppingItems构建过程：
     1）首先将ChargeParam中的List<ShoppingItem>转换为List<ShoppingCartItems>
	 2）下面的处理和buildByShoppingCart中通过购物车ShoppingCartid到数据库查到
	     List<ShoppingCartItems> goodsList后的处理过程相同
	
	