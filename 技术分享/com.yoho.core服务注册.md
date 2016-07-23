yoho.core服务注册
----
yoho.core的服务注册、发现基于Curator开源框架实现。
## 一、Curator介绍
#### Curator是Netflix开源的一套ZooKeeper客户端框架，简化和增强了zookeeper客户端编程。
#### 封装了ZooKeeper client与ZooKeeper server之间的连接处理，提供ZooKeeper各种应用场景(recipe, 比如共享锁服务, 集群领导选举机制)的抽象封装。
#### CuratorFramework，curator的一个模块，提供了一套高级的API， 简化了ZooKeeper的操作。 它增加了很多使用ZooKeeper开发的特性，可以处理ZooKeeper集群复杂的连接管理和重试机制。
#### Curator框架通过CuratorFrameworkFactory以工厂模式和builder模式创建CuratorFramework实例，CuratorFramework实例都是线程安全的，应该在应用中共享同一个CuratorFramework实例.
#### CuratorFramework实例创建后，调用start()启动，在应用退出时调用close()方法关闭。
## 二、yoho.core服务注册实现
### 1.定义了ServiceDesc注解
提供了三个属性：serviceName（服务名称默认为""）、value（服务名称默认为""）、degrade（是否降级）。
程序中通过spring的AnnotationUtils类解析注解。
### 2.在xml中配置相关自定义类
spring-rest-service.xml中配置了：
```xml
<bean id="serviceServerRegister" class="com.yoho.core.rest.server.zookeeper.ZookeeperRegister">
	<constructor-arg name="client" ref="curatorFramework"/>
	<constructor-arg name="basePath" value="/yh/services"/>
</bean>
<bean id="serviceDiscovery" class="com.yoho.core.rest.server.YHServiceDiscovery">
	<property name="context" value="${web.context}"/>
	<property name="register" ref="serviceServerRegister"/>
</bean>
```
curatorFramework在spring-core-common.xml中配置
```xml
<bean id="curatorFramework" class="com.yoho.core.common.curator.CuratorFrameworkFactoryBean" destroy-method="destroy">
	<property name="connectString" value="${zkAddress:zk01.yohoops.org:2181,zk02.yohoops.org:2181,zk03.yohoops.org:2181,zk04.yohoops.org:2181,zk05.yohoops.org:2181}"/>
</bean>
```
CuratorFrameworkFactoryBean负责curatorFramework的创建、启动与关闭。
```java
public void afterPropertiesSet() throws Exception {

    if(connectString  == null){
      return;
    }

    CuratorFrameworkFactory.Builder builder = CuratorFrameworkFactory.builder();

    builder.connectString(connectString);

    if (retryPolicy == null) {
      retryPolicy = new ExponentialBackoffRetry(1000, 3);
    }
    builder.retryPolicy(retryPolicy);

    if (sessionTimeout != null) {
      builder.sessionTimeoutMs(sessionTimeout);
    }

    if (namespace != null) {
      builder.namespace(namespace);
    }

    curator = builder.build();
    curator.start();
    logger.info("start zookeeper client success. connected to {}", connectString);
  }
```
ZookeeperRegister通过curatorFramework生成ServiceDiscovery进行服务示例ServiceInstance的注册。
YHServiceDiscovery负责服务的扫描和发现，并通过ZookeeperRegister注册服务。
### 3.服务注册具体过程
#### 3.1 服务标记
服务名格式为：AAA.BBB,AAA为在controller的类上的加ServiceDesc注解定义的名字，如果没加注解则为web.context.
BBB为@RequestMapping方法上加注解定义的名字，如果没加则为方法名。
#### 3.2 服务获取
在YHServiceDiscovery初始化时，通过ApplicationContext获取所有加了@Controller和@RestController的bean,放到serviceBeanMap中
```java
serviceBeanMap.putAll(applicationContext.getBeansWithAnnotation(Controller.class));
serviceBeanMap.putAll(applicationContext.getBeansWithAnnotation(RestController.class));
```
设置监听地址：listenAddress，格式ip:port,示例：127.0.0.1:8081，获取本机iP，根据web.context取端口号，各模块端口号写在代码里了：
```java
/** context --> port */
	private final  static Map<String, Integer> context_port_map = Maps.newHashMap();
	static {
		context_port_map.put("users", 8081);
		context_port_map.put("sns", 8082);
		context_port_map.put("product", 8083);
		context_port_map.put("order", 8084);
		context_port_map.put("promotion", 8085);
		context_port_map.put("message", 8086);
		context_port_map.put("resources", 8087);
		context_port_map.put("platform", 8088);
	}
```
##### 然后遍历serviceBeanMap：
##### 通过AnnotationUtils查找bean的ServiceDesc注解，有则serviceType=ServiceDesc.value,没有则serviceType=web.context
##### 查找bean的RequestMapping注解，获取该controller的访问路径，class_path=RequestMapping.value
##### 获取该bean上的所有方法methods，并遍历该methods：
##### 在每个method上获取RequestMapping注解，如何没获取到则continue处理下一个method（没有RequestMapping也就无法通过url访问到）
##### 有则method_path=RequestMapping.value
##### 获取该方法上的ServiceDesc注解，有则methodName=ServiceDesc.value，没有则methodName=方法名
##### 通过ZookeeperRegister.register注册该方法所提供的服务，参数如下：
	web.context
	listenAddress
	serviceType
	methodName
	method_path
	class_path
	method_anno
#### 3.3服务注册
ZookeeperRegister初始化
```java
 //JSON序列化
YHJsonInstanceSerializer<InstanceDetail> serializer = new YHJsonInstanceSerializer<InstanceDetail>(InstanceDetail.class);
serviceDiscovery = ServiceDiscoveryBuilder.builder(InstanceDetail.class)
		.client(client)
		.serializer(serializer)
		.basePath(basePath)
		.build();
serviceDiscovery.start();
```
	自定义serializer，默认是curator的JsonInstanceSerializer。
	InstanceDetail为自定义的服务描述，
	basePath=/yh/services为各服务的父节点。
	
```java	
boolean degrade = serviceDesc == null ? false : serviceDesc.degrade();
```

是否降级degrade=false 
通过register接收的参数构建InstanceDetail保存服务信息,
其中serviceName=serviceType.methodName
requestUrl=http://+linstenAddress+"/"+web.context+class_path+method_path

```java
private String buildRequestUrl() {
                URIBuilder builder = new URIBuilder(schema + instanceDetail.linstenAddress);
                builder.addPath(instanceDetail.context);
                builder.addPath(instanceDetail.controllerRequestMapping);
                builder.addPath(instanceDetail.methodRequestMapping);
                return builder.build();
        }
```
生成服务实例ServiceInstance
```java
ServiceInstance<InstanceDetail> serviceInstance = ServiceInstance.<InstanceDetail>builder()
                    .name(instanceDetail.getServiceName())
                    .address(listenAddress.split(":")[0])
                    .port(Integer.parseInt(listenAddress.split(":")[1]))
                    .payload(instanceDetail)
                    .build();
```
注册服务
```java
this.serviceDiscovery.registerService(serviceInstance);
```









