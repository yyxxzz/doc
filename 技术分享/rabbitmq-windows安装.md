1����װRabbitMQ��Ҫ�Ȱ�װErlang���Կ����������ص�ַ��
 http://www.erlang.org/downloads
 
�����õ�����汾��otp_win32_R16B03-1.exe 
Ȼ��next��next��next

      ���û������� ERLANG_HOME    C:\Program Files (x86)\erl5.10.4
      ��ӵ�PATH        %ERLANG_HOME%\bin;
2����װRabbitMQ 
���ص�ַhttp://www.rabbitmq.com/download.html
 
�����EXE����next��next��װ������ǽ�ѹ���棬��ѹ����
      ���û������� D:\rabbitmq\rabbitmq_server-3.6.1
      ��ӵ�PATH %RABBITMQ_SERVER%\sbin;
3������%RABBITMQ_SERVER%\sbin Ŀ¼�Թ���Ա������� 
cd D:\rabbitmq\rabbitmq_server-3.6.1\sbin
������:rabbitmq-service install
Ȼ��ᷢ��windows�Ѿ�װ��rabbitmq�ķ���������������
 
�����rabbitmq�Ŀ���̨��Ч��
"D:\rabbitmq\rabbitmq_server-3.6.1\sbin\rabbitmq-plugins.bat" enable rabbitmq_management

��������net stop RabbitMQ && net start RabbitMQ

������һЩ��sbinĿ¼�£������û���������ɫ�����������ļ�Ȩ�޵�
������һ����߲��������������Щ���п���̨Ϊ�β����أ���
cd D:\rabbitmq\rabbitmq_server-3.6.1\sbin
rabbitmqctl status
rabbitmqctl.bat list_users 
rabbitmqctl.bat list_vhosts 
rabbitmqctl.bat add_user yoho yoho

rabbitmqctl.bat set_user_tags yoho administrator 
rabbitmqctl.bat set_permissions -p /  yoho ".*" ".*" ".*"
4���ã������ڿ���̨�����û��������û���ɫ�������û������ļ�Ȩ�޵ȡ�

���������localhost:15672  Ĭ���˺ţ�guest  ���룺guest
��¼��admin�����Ǵ����û�����ɫ�������ļ�Ȩ�޵ȡ�
 
�����û�
 
��������������
 
Ȩ�ޣ�
 

ΪʲôҪ�Լ��ڱ��ذ�װһ���أ�
��Ϊ�����ò��Ի�����ʱ�򣬻ᷢ�ֱ��ص��Դ����ʱ�����ڲ��Ի�����consumer�ж��������������ʱ��message��һ����·�ɵ��Լ�������

5��ϢͶ�ݹ���
Rabbitmq����exchange,routing-key,�Լ�binding��˵����
Message��Ͷ��ʱ���Ƚ���ϢͶ�ݵ�exchange�У�Ȼ�����binding��Ϣ��exchange����binding�е�routing-key�ҵ���Ӧ�Ķ��С�






