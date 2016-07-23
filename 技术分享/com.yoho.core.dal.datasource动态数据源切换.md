com.yoho.core.dal.datasource多数据源集群动态切换实现逻辑
-----
## 一、大致思路

通过配置文件获取多数据源集群信息以及dao接口与数据源的对应关系并保存到MultiDataSourceRouter中，
定义类DynamicDataSource继承AbstractRoutingDataSource，实现determineCurrentLookupKey抽象方法（为获取链接的时候提供数据源key），
初始化每个dataSource,保存到DynamicDataSource.targetDataSources中，并设置默认的defaultTargetDataSource，
生成并注入sqlsessionFactory（dataSource=dynamicDataSource）。
定义拦截器在运行时调用dao方法时，通过Dao的信息获取dataSource的key，并设置到MultiDataSourceRouter.dataSourceKey中，
在Dao方法具体执行时，会调用determineCurrentLookupKey方法获取刚刚设置的key，从而取得数据源执行数据库操作。
在配置文件中通过AOP标签配置该拦截器，设置哪些包会被监控拦截。

## 二、具体过程
### 1.配置数据源信息
使用Yaml格式配置具体如下（order模块示例）：

    datasources:
      yh_orders:
        servers:
          - 192.168.102.219:3306
          - 192.168.102.219:3306
        username: yh_test
        password: 9nm0icOwt6bMHjMusIfMLw==
    
      yhb_operations:
        servers:
          - 192.168.102.219:3306
          - 192.168.102.219:3306
        username: yh_test
        password: 9nm0icOwt6bMHjMusIfMLw==
        daos:
          - com.yoho.yhorder.dal.IGateDAO
     
### 2.读取保存数据源信息	  
在spring-mybatis-datasource.xml中配置如下信息进行配置文件的读取并保存到databasesMap中：

	<bean id="databasesMap" class="org.springframework.beans.factory.config.YamlMapFactoryBean">
		<property name="resources">
			<list>
				<value>classpath:databases.yml</value>
			</list>
		</property>
	</bean>
	
### 3.数据源初始化及相关信息的保存
在spring-mybatis-datasource.xml中配置如下信息：

	<bean id="dataSourceBeanFactory" class="com.yoho.core.dal.datasource.DataSourceBeanFactory" init-method="init" >
		<property name="defaultDBUser" value="${db.default.user:yh_vpc_bak}"/>
		<property name="defaultDBPassword" value="${db.default.password:+BfhVxZQ4LuPKt+QxSy9naMwEu/zaKV31I9S8xDJIUA=}"/>
		<property name="databasesMap" ref="databasesMap"/>
	</bean>
	
### com.yoho.core.dal.datasource.DataSourceBeanFactory的init方法进行初始化具体过程如下：
#### 3.1解析配置信息
从databasesMap中取出key=datasources的数据源信息，遍历处理后生成的信息有：

- dataSource：
    配置文件中的servers下的每个ip都会生成一个dataSource，连接池配置等信息在代码中写死，其他设置信息为beanId=schema@host，
    如上述配置文件中第一schema=yh_orders，该schema有两个dataSource，beanId分别为yh_orders@192.168.102.219:3306，yh_orders@192.168.102.219:3306，每个schema是一个集群保存中Map<String, String> dbClusterSet中。
- Map<String, String> dbClusterSet：key=schema，value=beanId，beanId，beanId...
- Map<String, String> daoDbClusterMap：key=具体dao，value=schema
- DefaultDBCluster：默认集群，没有指定schema的Dao默认是第一个schema，上述配置文件中是yh_orders。
- dbClusterSet、daoDbClusterMap、DefaultDBCluster信息保存到MultiDataSourceRouter动态路由对象中。
- 各个dataSource对象实例保存到List<DataSource> dataSources中。

#### 3.2生成并注入动态数据源
#### 3.2.1定义com.yoho.core.dal.datasource.DynamicDataSource继承AbstractRoutingDataSource实现determineCurrentLookupKey抽象方法，

	@Override
	protected Object determineCurrentLookupKey() {
		return MultiDataSourceRouter.getCurrentDataSourceKey();
	}
	
拦截器在调用dao的方法前会利用DAO的信息通过MultiDataSourceRouter获取到DataSourceKey并设置在MultiDataSourceRouter中，
当拦截器invorkDao的方法时会回调determineCurrentLookupKey方法从而路由到正确的DataSource。
#### 3.2.2生成并注入DynamicDataSource
设置beanid=dynamicDataSource，
初始化每个dataSource放入targetDataSources中，
targetDataSources=targetDataSources
#### 3.2.3生成并注入sqlsessionFactory
实现如下配置：

	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="dynamicDataSource"/>
		<property name="mapperLocations" value="classpath*:META-INF/mybatis/*.xml"></property>
	</bean>
#### 3.3设置ReadOnlyInSlave

### 4.定义拦截器实现运行时动态路由
配置信息如下：

    <!--数据库访问拦截器， 数据库访问延时统计-->
	<bean id="dataSourceMethodInterceptor" class="com.yoho.core.dal.datasource.intercepor.DaoInterceptor">
	</bean>

	<aop:config>
		<aop:pointcut id="daoPoint"
					  expression="execution(* com.yoho.*.dal.*.*(..)) or execution(* com.yohobuy.platform.dal.*.*.*(..)) "/>
		<aop:advisor pointcut-ref="daoPoint" advice-ref="dataSourceMethodInterceptor"/>
	</aop:config>
DaoInterceptor拦截器获取到DAO的类名和statementID，然后执行如下
  
  //数据源路由
	String dataSource = MultiDataSourceRouter.router(mapperNamespace, statementId)
	
	MultiDataSourceRouter.setDataSourceKey(dataSource)
	
在具体数据库操作时（invocation.proceed()）会回调DynamicDataSource的determineCurrentLookupKey方法从而路由到正确的DataSource。	

