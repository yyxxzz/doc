限购码管理
LimitCodeController
1、	该类中的方法通过@RequestMapping(" ")中url的方式与页面进行数据交互；

2、	当获取数据库中的数据的请求产生时，该类中方法的的形参为Dal层对象，如本业务中的LimitCodeReq；当向数据库中写入或者修改数据的请求产生时，该类中方法的形参为Model层对象，如本业务中的LimitCodeBo；

3、	该类方法下对于页面请求数据的获取通过调用Service对象中的方法实现，Service为业务逻辑层面，页面的增删改查等具体业务需求方法定义于Service层中，如：
// 查询总数          Integer totalNum = iLimitCodeService.getLimitCodeCount(limitCodeReq);
其中，getLimitCodeCount(LimitCodeReq limitCodeReq)为Service层中定义的方法，limitCodeReq为dal层方法，用来获取查询结果数据。

4、	该类中获取的所有数据通过Service中的方法中的参数与Dal层进行数据交互；

5、	Service的对象中的方法与Dal层中Mybatis下的.xml文件中sql语句的id相对应，该id为具体业务需求确定进行相应sql语句的编写，如：
iLimitCodeService 中的getLimitCodeCount(LimitCodeReq limitCodeReq)方法与以下mybatis中的sql相对应
 

6、	该类中的方法的返回类型为model层通用的响应类ApiResponse，该响应类中进行与页面响应格式的构造，响应数据的传递，响应状态的返回等；

7、	在该类中编写方法时，不能盲目地将获取的请求进行处理，要先判断请求是否有效，或者数据是否为空，要注意输出相应日志信息便于调试查看，如：
 

