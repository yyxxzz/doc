1：安装RabbitMQ需要先安装Erlang语言开发包。下载地址：
 http://www.erlang.org/downloads
 
笔者用的这个版本：otp_win32_R16B03-1.exe 
然后next—next—next

      配置环境变量 ERLANG_HOME    C:\Program Files (x86)\erl5.10.4
      添加到PATH        %ERLANG_HOME%\bin;
2：安装RabbitMQ 
下载地址http://www.rabbitmq.com/download.html
 
如果是EXE，就next—next安装，如果是解压缩版，解压缩。
      配置环境变量 D:\rabbitmq\rabbitmq_server-3.6.1
      添加到PATH %RABBITMQ_SERVER%\sbin;
3：进入%RABBITMQ_SERVER%\sbin 目录以管理员身份运行 
cd D:\rabbitmq\rabbitmq_server-3.6.1\sbin
敲命令:rabbitmq-service install
然后会发现windows已经装了rabbitmq的服务啦，启动服务。
 
下面把rabbitmq的控制台生效：
"D:\rabbitmq\rabbitmq_server-3.6.1\sbin\rabbitmq-plugins.bat" enable rabbitmq_management

重启服务：net stop RabbitMQ && net start RabbitMQ

下面是一些在sbin目录下：创建用户，创建角色，创建所属文件权限等
（但是一般笔者不会用命令操作这些，有控制台为何不用呢？）
cd D:\rabbitmq\rabbitmq_server-3.6.1\sbin
rabbitmqctl status
rabbitmqctl.bat list_users 
rabbitmqctl.bat list_vhosts 
rabbitmqctl.bat add_user yoho yoho

rabbitmqctl.bat set_user_tags yoho administrator 
rabbitmqctl.bat set_permissions -p /  yoho ".*" ".*" ".*"
4：好，下面在控制台创建用户，授予用户角色，创建用户所属文件权限等。

浏览器访问localhost:15672  默认账号：guest  密码：guest
登录：admin这里是创建用户，角色，所属文件权限等。
 
创建用户
 
创建虚拟主机：
 
权限：
 

为什么要自己在本地安装一个呢？
因为笔者用测试环境的时候，会发现本地调试代码的时候，由于测试环境上consumer有多个，所以在消费时，message不一定能路由到自己本机。

5消息投递过程
Rabbitmq中有exchange,routing-key,以及binding的说法。
Message在投递时会先将消息投递到exchange中，然后根据binding信息，exchange根据binding中的routing-key找到对应的队列。






