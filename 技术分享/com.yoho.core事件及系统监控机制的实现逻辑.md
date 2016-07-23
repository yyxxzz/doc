yohobuy事件及系统监控机制的实现逻辑
----
## 一、大致思路

 基于spring事件机制，根据需要监控的内容（数据库操作、rabitMQ、服务调用、log、异常等）定义各类事件和监听器，通过AOP或者直接方式捕获并发布事件，各监听器处理事件并通过包装在监听器内部的EventReporter报告处理过的事件并通过InfluxDB将信息保存到数据库中，供各相关系统查询。

## 二、具体实现
### 1.spring事件机制
#### Spring事件体系包括一下组件：
- 事件：ApplicationEvent
- 事件监听器：ApplicationListener，对监听到的事件进行处理,具体的监听器要该接口onApplicationEvent方法。
- 事件广播器：ApplicationEventMulticaster，有保存所有事件监听器的监听器注册表，负责将Springpublish的事件广播给所有的监听器。
- 事件发布者：ApplicationEventPublisher,spring提供了默认实现AbstractApplicationContext

#### 初始化：

在容器启动的时候，AbstractApplicationContext会首先找有没有用户配置的事件广播器ApplicationEventMulticaster，\n
如果有则加载进来，没有则new一个事件广播器SimpleApplicationEventMulticaster。具体代码如下：
```java
 protected void initApplicationEventMulticaster() {
		ConfigurableListableBeanFactory beanFactory = getBeanFactory();
		if (beanFactory.containsLocalBean(APPLICATION_EVENT_MULTICASTER_BEAN_NAME)) {
			this.applicationEventMulticaster =
					beanFactory.getBean(APPLICATION_EVENT_MULTICASTER_BEAN_NAME, ApplicationEventMulticaster.class);
			if (logger.isDebugEnabled()) {
				logger.debug("Using ApplicationEventMulticaster [" + this.applicationEventMulticaster + "]");
			}
		}
		else {
			this.applicationEventMulticaster = new SimpleApplicationEventMulticaster(beanFactory);
			beanFactory.registerSingleton(APPLICATION_EVENT_MULTICASTER_BEAN_NAME, this.applicationEventMulticaster);
			if (logger.isDebugEnabled()) {
				logger.debug("Unable to locate ApplicationEventMulticaster with name '" +
						APPLICATION_EVENT_MULTICASTER_BEAN_NAME +
						"': using default [" + this.applicationEventMulticaster + "]");
			}
		}
```	
然后spring会通过反射机制将所有实现了事件监听器接口ApplicationListener的bean注册到事件广播器的监听器注册表中。
    
#### 事件发布和处理：

只要是实现了ApplicationEventPublisherAware接口的bean，spring会将事件发布者实际就是ApplicationContext设置到该bean中。
要发布事件就调用ApplicationEventPublisher的publishEvent方法，在该方法中spring会委托事件广播器发布事件。
事件广播器会遍历注册的每个监听器，并启动来调用每个监听器的onApplicationEvent方法进行事件的处理。
由于spring自动new的事件广播器SimpleApplicationEventMulticaster的taskExecutor为null，因此，事件监听器对事件的处理，是同步进行的。
要等所有监听器处理完事件才能返回。
```java
    @Override
	public void multicastEvent(final ApplicationEvent event, ResolvableType eventType) {
		ResolvableType type = (eventType != null ? eventType : resolveDefaultEventType(event));
		for (final ApplicationListener<?> listener : getApplicationListeners(event, type)) {
			Executor executor = getTaskExecutor();
			if (executor != null) {
				executor.execute(new Runnable() {
					@Override
					public void run() {
						invokeListener(listener, event);
					}
				});
			}
			else {
				invokeListener(listener, event);
			}
		}
	}
	
	@SuppressWarnings({"unchecked", "rawtypes"})
	protected void invokeListener(ApplicationListener listener, ApplicationEvent event) {
		ErrorHandler errorHandler = getErrorHandler();
		if (errorHandler != null) {
			try {
				listener.onApplicationEvent(event);
			}
			catch (Throwable err) {
				errorHandler.handleError(err);
			}
		}
		else {
			listener.onApplicationEvent(event);
		}
	}
```

如果想用异步的，可以自己实现ApplicationEventMulticaster接口，并在Spring容器中注册id为 **applicationEventMulticaster** 的Bean。
Spring发布一个事件之后，所有注册的事件监听器，都会收到该事件，因此，事件监听器在处理事件时，需要先判断该事件是否是自己关心的。
    
### 2.yoho.core的实现
- 定义CommonEvent，扩展自ApplicationEvent增加了name属性，在com.yoho.error.event包中。
- 定义了各个具体事件（DatabaseAccessEvent、GatewayAccessEvent、ServiceCallEvent等）都继承自CommonEvent。
- 定义抽象类AbstractEventHandler并实现ApplicationListener接口的onApplicationEvent方法进行各事件的公共处理。
- 针对各个事件定义对应的事件监听器（DatabaseAccessEventHandler、GatewayAccessEventHandler等）都继承自AbstractEventHandler。
- 各个需要发布事件的bean实现ApplicationEventPublisherAware接口，调用ApplicationEventPublisher.publishEvent()发布事件。
- 由于是异步的，所以要实现ApplicationEventMulticaster接口，故在spring-core-alarm.xml中定义了如下bean,id必须是**applicationEventMulticaster**：

```xml
    <!--支持异步的事件-->
    <bean id="applicationEventMulticaster"
          class="org.springframework.context.event.SimpleApplicationEventMulticaster">
        <property name="taskExecutor">
            <bean class="org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor"/>
        </property>
    </bean>
```
- 各监听器中在xml都设置了eventReporter属性，示例如下：

```xml
    <bean id="databaseAccessEventHandler" class="com.yoho.core.common.alarm.event.DatabaseAccessEventHandler">
        <property name="eventReporter" ref="eventReporter"/>
        <property name="tag_context" value="${web.context:default}"/>
    </bean>
```
- eventReporter定义如下：

```xml
    <bean id="eventReporter" class="com.yoho.core.common.alarm.InfluxDataReporter" init-method="init">
       <property name="dbName" value="yoho-monitor"/>
        <property name="dbUrl" value="${alarm.influxdb.url:http://influxdb.yohoops.org:8086}"/>
        <property name="user" value="${alarm.influxdb.user:root}"/>
        <property name="password" value="${alarm.influxdb.password:root}"/>
        <property name="enable" value="${alarm.influxdb.enable:true}"/>
    </bean>
```
- InfluxDataReporter实现了EventReporter自定义接口，在init初始化方法中创建了InfluxDB链接并设置了一些参数。
- 实现report方法，进行具体的InfluxDB写入。
- 要使用InfluxDB功能，pom中要加入

```xml
    <dependency>
		<groupId>org.influxdb</groupId>
		<artifactId>influxdb-java</artifactId>
		<version>2.1</version>
	</dependency>
```
