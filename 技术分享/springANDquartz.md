spring集成quartz
注意：出现异常“Caused by: java.lang.IncompatibleClassChangeError: class org.springframework.scheduling.quartz.CronTriggerBean has interface org.quartz.CronTrigger as super class”
Spring3.0不支持Quartz2.0,因为org.quartz.CronTrigger在2.0从class变成了一个interface造成IncompatibleClassChangeError错误:

有两种办法：
第一种降低quartz的版本为，最好是quartz1.8
第二种是升级spring版本为Spring3.2以上
注：Spring3.2.4配置文件中使用CronTriggerFactoryBean来集成quartz2.x,使用CronTriggerBean来集成quartz1.8.x及以前版本.
 
    以下是一个小例子demo
    #################################### 启动触发器的配置开始 #####################################
    <bean name="startQuertz" lazy-init="false" autowire="no"  
        class="org.springframework.scheduling.quartz.SchedulerFactoryBean">  
        <property name="triggers">  
            <list>  
                <ref bean="myJobTrigger" />  
            </list>  
        </property>  
    </bean>
    #################################### 启动触发器的配置结束 #####################################
  
    ###### 调度的配置开始 
    #################################### quartz-1.8以前的配置 #####################################
           
    <bean id="myJobTrigger"  
        class="org.springframework.scheduling.quartz.CronTriggerBean">  
        <property name="jobDetail">  
            <ref bean="myJobDetail" />  
        </property>  
        <property name="cronExpression">  
            <value>0/1 * * * * ?</value>  
        </property>  
    </bean>  
    #################################### quartz-1.8以前的配置 #####################################
    
    ####################################  quartz-2.x的配置 #####################################
        <bean id="myJobTrigger"  
        class="org.springframework.scheduling.quartz.CronTriggerFactoryBean">  
        <property name="jobDetail">  
            <ref bean="myJobDetail" />  
        </property>  
        <property name="cronExpression">  
            <value>0/1 * * * * ?</value>  
        </property>  
    </bean>
    ####################################  quartz-2.x的配置 #####################################
     
  
    ####################################  job的配置开始 #####################################
    <bean id="myJobDetail"  
        class="org.springframework.scheduling.quartz.MethodInvokingJobDetailFactoryBean">  
        <property name="targetObject">  
            <ref bean="myJob" />  
        </property>  
        <property name="targetMethod">  
            <value>work</value>  
        </property>  
    </bean>
    #################################### job的配置结束 #####################################
    
  
   
    #################################### 工作的bean #####################################
    <bean id="myJob" class="com.tgb.lk.demo.quartz.MyJob" />  
    


package com.demo.test;
import java.util.Date;

/*
 * 使用spring+Quartz执行任务调度的具体类
 * */
public class MyJob {
    /*
     * Description:具体工作的方法，此方法只是向控制台输出当前时间，
     * 输入的日志在：%tomcatRoot%\logs\tomcat7-stdout.yyyy-MM-dd.log中，
     * 其中，yyyy-MM-dd为部署的日期,经试验发现默认情况下并不是每天都生成一个stdout的日志文件
     * @return 返回void
     * */
    public void work()
    {
         System.out.println("当前时间:"+new Date().toString());  
    }
}