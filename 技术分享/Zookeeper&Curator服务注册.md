# Zookeeper & Curator 服务注册

curator是最简单的Zookeeper客户端
####Curator主要组件
+ Recipes   (扩展:包括分布式锁、队列、选举等)
+ Framework  (框架)
+ Utilities  (工具)
+ Client (客户端)
+ Errors  (错误处理)

另外Curator提供了一些扩展库，比如用于服务注册的curator-x-discovery，yohobuy_core模块中核心的服务注册机制也是基于curator的discovery库。

之前写过yoho的服务注册和发现模块的大致流程和类图分析，但未涉及到具体的服务注册过程，本文主要对服务注册进行简单的学习。
首先主要学习的是使用Curator进行简单的服务注册与发现，后面结合yohobuy_core的服务注册实现进一步学习。

### 服务定义  ServiceInstance
Curator 中可以使用ServiceInstance作为服务定义对象
通常一个服务对象具有服务名、服务id、地址、端口和有效数据等属性，
下面就是ServiceInstance类的变量定义。
```java
public class ServiceInstance<T> {
    private final String name;
    private final String id;
    private final String address;
    private final Integer port;
    private final Integer sslPort;
    private final T payload;
    private final long registrationTimeUTC;
    private final ServiceType serviceType;
    private final UriSpec uriSpec;
    ...
}
```
服务注册过程就是将ServiceInstance实例写到Zookeeper层次结构中，服务名name会生成一个ZkNode,id会生成这个name下孩子节点，因为同一个服务名下面可以有很多具体的服务实例，所以这种情况下会有很多个id。在注册的时候会指定根路径，所以注册之后的服务在zookeeper中如下图所示
```
根路径(注册时指定)
|_____name1 //服务名1
|        |______id1->序列化ServiceInstance1
|		|______id2->序列化ServiceInstance2
|
|_____name2 //服务名2
|        |______id1->序列化ServiceInstance1
|		|______id2->序列化ServiceInstance2
|
```
可以先通过客户端连接到本地zookeeper查看下yoho的服务注册情况，其注册在/yoho/services/路径下，会发现有很多服务，再进服务查看可以看到这个服务下有多少个实例，其节点为其id(yoho_core中服务未设置id，使用的是默认生成的一大串字符串),通过get命令可以查看节点的存储数据，序列化后的服务对象信息。

要注意到是服务在注册时生成的name服务名节点是持久的节点，而服务名下面的实例id节点是临时节点，在注册服务的程序退出之后，其也会消失。

构造服务实例的方法很简单，可以通过ServiceInstance的build方法进行建造。
在练习中定义了一个简单的服务业务类XServInstance
``` java
public class XServInstance implements Serializable{
    private  String id;
    private  String name;
    private  String path;
    get&set()...
}

```
建造ServiceInstance的代码如下:
```java
ServiceInstance<XServInstance> serviceInstance= ServiceInstance.<XServInstance>builder()
                .id(instance.getId())
                .name(instance.getName())
                .payload(instance).build();

```
### 服务注册  ServiceDiscovery
Curator使用ServiceDiscovery进行服务注册，可以把它认为是服务管理中心，通过它也能查找我们的服务。ServiceDiscovery同样采用建造者方式，由ServiceDiscoveryBuilder进行构造,在构造完成之后需要调用start方法！SERV_PAHT指定我们服务需要注册在zookeeper的哪个目录
```java
JsonInstanceSerializer<XServInstance> serializer=new JsonInstanceSerializer<XServInstance>(XServInstance.class);
serviceDiscovery= ServiceDiscoveryBuilder.builder(XServInstance.class)
                .basePath(SERV_PATH)
                .client(client)
                .serializer(serializer)
                .build();
serviceDiscovery.start();

```
client是curator的zookeeper客户端，通过下面方式构造:
```java
RetryPolicy retryPolicy=new RetryOneTime(1000);
//CONNECT_STR : "127.0.0.1:2181"
client= CuratorFrameworkFactory.newClient(CONNECT_STR,retryPolicy);
client.start()//必须调用

```
注册服务的代码就一行:
```java
serviceDiscovery.registerService(serviceInstance);
```
此时服务已注册，在程序未退出时查看SERV_PATH下的内容即注册服务serviceInstance的name，再下一层就是id了，如果同一个服务注册了多个，则会有多个id(注册id相同只会有一个)。退出程序之后，由其注册的id也就没了。
ServiceDiscovery除了注册服务，同时也能查询服务，其三个主要的查询方法如下:

+  Collection<String> queryForNames()  //查询所有服务名
+  Collection<ServiceInstance<T>>  queryForInstances(String name)  //查询指定服务名下的所有服务实例
+  ServiceInstance<T> queryForInstance(String name, String id)      //根据服务名和id查询服务实例




### 服务提供  ServiceProvider
这一步是Curator自带的服务提供方式，在yoho_core中并未采用自带的服务提供者，而是通过ServiceDiscovery查询(queryForInstances)到服务之后，通过自己的策略进行服务选择(后期待续)。这里直接使用curator现成的服务提供者。

服务提供者的构造通过serviceDiscovery内部的建造者方法进行构造，同样必须先start()
需要注意的是providerStrategy的提供策略，对服务提供者设置一个服务选择策略，因为同一个服务名下有很多具体服务实例，具体选择哪一个可以由该策略决定：
常用的有RoundRobinStrategy 轮询，RandomStrategy 随机选择 ，StickyStrategy 粘性策略(总是选择同一个，没试过比较特殊，可能适用于某些服务调用者和提供者需要记录什么调用记录的情况下吧)
```java
ServiceProvider servPro = serviceDiscovery.serviceProviderBuilder()
		.serviceName(servName)
		.providerStrategy(new RoundRobinStrategy<XServInstance>())
		.build();
servPro.start();
```
获取一个服务通过以下方式进行获取
```java
ServiceInstance<XServInstance> serviceInstance=servPro.getInstance();
```

yoho_core内的CuratorXDiscoveryClientWrapper用来选择服务，其用到了ServiceCache，本文描述的方式不同，待下次学习分解...

最后附上本练习简单的服务注册中心代码和测试代码
## XServCenter
```java
public class XServCenter {
    public  static final  String SERV_PATH="/myServices";
    ServiceDiscovery<XServInstance> serviceDiscovery ;
    Map<String,ServiceProvider> providerMap=new HashMap<String, ServiceProvider>();

    public  void init(CuratorFramework client) throws Exception {
        JsonInstanceSerializer<XServInstance> serializer=new JsonInstanceSerializer<XServInstance>(XServInstance.class);
        serviceDiscovery= ServiceDiscoveryBuilder.builder(XServInstance.class)
                .basePath(SERV_PATH)
                .client(client)
                .serializer(serializer)
                .build();
        serviceDiscovery.start();
    }

    public  void regist(XServInstance instance) throws Exception {
        ServiceInstance<XServInstance> serviceInstance= ServiceInstance.<XServInstance>builder()
                .id(instance.getId())
                .name(instance.getName())
                .payload(instance).build();
        serviceDiscovery.registerService(serviceInstance);

    }

    public  void list() throws Exception {
        Collection<String> servNames=serviceDiscovery.queryForNames();
        for(String servName:servNames){
            Collection<ServiceInstance<XServInstance>> instances=serviceDiscovery.queryForInstances("/"+servName);
            System.out.println(servName);
            for(ServiceInstance<XServInstance> instance:instances){
                System.out.println("------- : "+instance.getId());
            }

        }
    }

    public  void getServiceByName(String servName) throws Exception {
        ServiceProvider servPro =providerMap.get(servName);
        if (servPro==null) {
            servPro = serviceDiscovery.serviceProviderBuilder()
                    .serviceName(servName) //RandomStrategy StickyStrategy
                    .providerStrategy(new RoundRobinStrategy<XServInstance>())  
                    .build();
            servPro.start();
            providerMap.put(servName,servPro);
        }
        ServiceInstance<XServInstance> serviceInstance=servPro.getInstance();
        System.out.println("getServiceByName  "+serviceInstance.getId());
    }

```

## Test
```java
public class ServTest {
    public  static  final String CONNECT_STR="127.0.0.1:2181";
    public static CuratorFramework client;
    public  static  void main(String args[]) throws Exception {
        RetryPolicy retryPolicy=new RetryOneTime(1000);
        client= CuratorFrameworkFactory.newClient(CONNECT_STR,retryPolicy);
        if(client==null) return;
        client.start();
		
		Runtime.getRuntime().addShutdownHook(new Thread(){
            public void run() {
                CloseableUtils.closeQuietly(client);
            }
        });
		
        XServCenter xServCenter=new XServCenter();
        xServCenter.init(client);
        for(int i=0;i<10;i++) {
            for(int j=0;j<10;j++) {
                XServInstance xServInstance = new XServInstance();
                xServInstance.setId("serv_id_" + i+"_"+j);
                xServInstance.setName("serv_name_" + i);
                xServCenter.regist(xServInstance);
            }
        }
        System.out.println("#####list######");
        xServCenter.list();

        for(int i=0;i<1000;i++) {
            xServCenter.getServiceByName("serv_name_1");
            Thread.sleep(500);
        }
		
        Thread.sleep(600000);
      
    }
}
```






