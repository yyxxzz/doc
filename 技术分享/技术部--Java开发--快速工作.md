技术部--Java开发--快速工作
------------

## 安装Java
Java版本选择8


## 安装Maven
安装完成之后，需要和老同事找setting文件


## 安装Navicat for MySQL：
安装Mysql数据库客户端软件
数据库配置信息：192.168.50.69:9980  yh_test/密码问同事


## 安装Git 和 SourceTree：
安装`《Git-2.6.2-64-bit.exe》`，公司GIT地址：`git.dev.yoho.cn`，需要根据账号让组长加项目权限
安装公司要求的Git客户端软件`《SourceTreeSetup_1.6.21》`
然后在网页GIT中获得工作相关项目的连接，如：`git@git.dev.yoho.cn:yoho30/yoho-core.git`，在SourceTree中“克隆/新建”对应的项目


## 安装Java IDE
安装`《ideaIU-14.1.5.exe》`，安装完成之后根据个人情况，一般需要设置如下参数：
`File-->Setting：Git，Maven(home,setting,repository)，KeyMap(快捷键风格同eclipse)，file encodings(IDE Encode,Default File Encoding)，Show line numbers`等等
在idea中导入Maven项目
增加Service的配置，配置完成之后再Service中增加web应用
公司代码规范见：`http://git.dev.yoho.cn/yoho-documents/api-interfaces/blob/master`


## 需求跟踪系统
公司需求跟踪系统redmine：`http://redmine.yoho.cn`，需要让组长加各项目需求管理的权限


## 下面以一个需求为例，描述项目搭建调试的过程
### 需求描述
现有功能：App端—>逛—>搭配：获得这个主题下面的10篇文章。
参照此功能，其他都一样，只是将后台返回的文章 换成 星潮教室 主题里面的文章。

### 找到现有功能App发出的URL地址：
安装fiddler4setup.exe工具(用于监控手机APP实际请求的URL，报文头，返回结果等信息)，
`注意生产环境YOHOAPP的Android版本是https请求的，看不到URL，需要把手机的yohoApp请app开发人员搞成测试环境的才可以的`安装好之后需要如下设置：
`Tools-->Fiddle Options-->Connections-->Allow remote computers to connect 并设置Fiddler listens on port:8888 ` 
`手机连接到wifi:YOHO-AP-->并作代理设置-->服务器:本机ID-->端口8888`
这样在手机yohoApp中的操作，就可以在Fiddler中看到请求的所有信息了.

### 根据App发出的URL地址，找到gateway接口和后台服务接口
找到上面Fiddler中记录的请求的URL为：`http://testservice.yoho.cn:28077/guang/api/v2/article/getList?app_version=4.1.0....其他参数`在gateway项目中找到对应的Controller，
根据需求在此Controller中新建一个接口。
根据gateway中Controller中调用服务service.call()的地址CATEGORY_LOAD_URL = "guang.getList"找到服务项目sns中的restapi接口，最终就可以找Service，Dao了：

### 根据需求建立文章标签表article_tags的Dao文件(有可能有的需求不需要)
利用《mybatis_generator》工具，根据连接的数据库信息，创建对应表的DO,IDAO,Mapper文件，修改工具中《generatorConfig.xml》即可。
注意要将最后生成的IDao文件，修改成符合公司编码规范的文件名“IArticleDAO”，并将文件放到对应项目的合适路径中
PS：如果要新增加DAO的接口，接口名称中不用带表名，如以ID查询article，直接编写selectById，不用selectArticleById，因为公司要求，一个DAO直接操作一个表。
最后根据业务需求编写SNS项目中的Service接口，restApi接口，gateway项目中的编写对外暴露的Controller API.


## 调试本机SNS项目
### 安装并启动zookeeper
在本机环境中安装zookeeper，只需要将下载的安装包解压，创建一个默认的配置文件即可(依据.\conf\zoo_sample.cfg,创建一个zoo.cfg即可)，启动zookeeper(.\bin\zkServer.cmd)

### 安装PostMan工具
Postman是一款功能强大的网页调试与发送网页HTTP请求的Chrome插件，用于模拟我们SNS本机项目的rest请求。 
在Chrome中的网上应用店中(需要先翻墙)搜索postman，并完成安装。
注意这里有一个坑，一定要保证Chrome的版本是比较高的(最好最新)，不然postman启动好了之后，不能正常启动。

### 开始调试SNS项目
先保证本机环境的zookeeper已经启动
在IDEA中，以调试的方式启动SNS项目，并在合适的地方打断点
再打开postman，以post方式请求，请求Body中选择 raw  JSON(application/json)，并在JSON对象的输入框中输入请求的JSON格式的参数。
Rest请求一切准备完毕，就可以点击最左边的Send按钮了。
最后就可以看到在IDEA中，已经出现调试界面，做业务方面的代码调试即可。
由于这个需求中新编写了IDAO，一般而言项目调试在请求到Dao的时候，会报一个`Table *** 不存在的错误`，
这个时候需要在`yoho-sns\web\src\main\resources\databases.yml`和`yoho-sns\web\src\main\webapp\META-INF\autoconf\databases.yml`中对应的数据库中增加上对应的IDAO接口，切记都要修改，不然发布到DEV等环境中，就挂了。
如果项目能够正常的调试完成，接口返回结果，在postman中是可以看到返回结果的
PS:如果自己调试的项目所引用的Maven包有更新，需要新拉取Respository，再IDEA中，项目右键Maven—》Reimport
PS:可能各个项目的zookeeper的配置不一样，需要保证调用本机的zookeeper的配置文件，都修改正确。 配置一般在`config.properties`文件中。


## 调试本机gateway项目
Gateway中的Controller接口编写完成之后，启动gateway项目。
启动postman(浏览器直接URL请求也可以)，书写正确的请求URL(带参数)，以get方式提交请求，并观察返回结果。
PS这里有一个坑，可能是由于模拟app发送的请求，并且框架做了拦截，导致请求到不了Controller，后台在拦截器代码中回报[500 数据验证错误]，这个需求在请求URL中增加debug标示：&debug=XYZ，


## 提交自己编写的代码
本机环境接口验证完成之后，千万不要忘记提交代码到Git，注意先提交本地，在推送到服务器。
注意提交之前先拉取，避免不可见的冲突。
PS：不推荐很长时间一次性提交太多的代码，尽量将自己的代码以完整体量方式多次提交，避免很多天之后一次性提交过多的代码，这样可以尽量小的避免冲突的发生，也可以让领导看到自己的工作量，但是不能提交不完整不能编译的代码。


## 按照规范要求，在yoho_Document中提交接口规范文档，gateway接口 


## DEV环境联调
### 自己4030代码提交后，合并分支的过程： 0430 和 合并到dev
1：打开0430分支，拉取一下
2：打开dev分支，拉取一下
3：确保目前打开的分支是dev分支，右键0430分支，合并到当前分支，OK 完成。
4：不要相信DEV分支，不要相信DEV分支，删除重搞！！！！！！

### 将0430合并后的DEV接口发布到205DEV开发环境
1：SSH到192.168.102.220机器 dev/密码问组长
2：进入目录并运行程序  /home/dev/opps/deploy/   ./run.sh
   根据不同项目的代号，进行部署更新，如：15/1
   有可能会发布不成功，原因是编译失败，一般原因是git自动编译文件问题。发布成功之后，205机器的对应项目.ZIP文件，日期肯定是更新的。
3：进入192.168.102.205机器 root/密码问组长,进行接口地址的更新
   1）method方式： /home/nginx/conf/redirect.lua  增加新增加的接口
   2）URL方式：    /home/nginx/conf/nginx.conf    后面没记住，反正guang的接口都是可以的
4：加完之后，进入  /home/nginx/sbin/nginx -s reload,这里没太懂

### 验证205DEV开发环境的接口: DEV
1：调用gateway：method方式，需要配置redirect.lua
   (如：sns以前可用)http://devapi.yoho.cn:58078/?method=web.favorite.product&app_version=3.8.2&client_type=android&os_version=yohobuy%3Ah5&screen_size=720x1280&v=7&uid=3407014&limit=10&debug=XYZ
1：调用gateway：非method方式，需要配置nginx
   (如：sns新开发可用)http://devservice.yoho.cn:58077/guang/api/v2/article/getStarClassroomArticleList?app_version=4.1.0&page=1&debug=XYZ
   (如：sns以前可用)http://devservice.yoho.cn:58077/guang/api/v2/article/getList?app_version=4.1.0.1603120001&client_secret=0955a436492e71767d4b9a825a367799&client_type=iphone&gender=2%2C3&limit=20&os_version=9.2.1&page=1&screen_size=320x568&sort_id=0&udid=258e608815a77ba2f9094da936f896ef4fd65721&uid=8039983&v=7&yh_channel=2
   (如：resource可用)http://devservice.yoho.cn:58077/operations/api/v5/resource/get?content_code=b38b9c4f1c76f89533e9214629b458e4&debug=XYZ
2：内在地址
   (Get方式)http://192.168.102.205:8080/gateway/guang/api/v2/article/getList?app_version=4.1.0.1603120001&client_secret=0955a436492e71767d4b9a825a367799&client_type=iphone&gender=2%2C3&limit=20&os_version=9.2.1&page=1&screen_size=320x568&sort_id=0&udid=258e608815a77ba2f9094da936f896ef4fd65721&uid=8039983&v=7&yh_channel=2
   (Post方式)http://192.168.102.205:8082/sns/ArticleRest/getList

### 如果发现问题，通过如下方式排查
1：看JAR包中的文件是否最新的：
   /home/dev/yoho-gateway/yoho-gateway.zip(./deploy是解压之后的目录) 和 /home/dev/yoho-gateway/deploy/gateway-war/WEB-INF/lib/yoho-gateway-service-1.0.0-SNAPSHOT.jar
   看class文件，是否和实际版本一样的。
2：查看日志啊啊啊啊啊
   /Data/logs/gateway/warn-log.log  和  /Data/logs/sns/debug.log和warn.log
   PS：logger.debug这种不打印日志，logger.info是打印日志的，这种日志在《debug-log.log》文件中，如果有异常错误在《warn.log》文件中。


## 灰度环境中验证接口(还未开始，带完成后继续完善 ) 

