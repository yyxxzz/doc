������--Java����--���ٹ���
------------

## ��װJava
Java�汾ѡ��8


## ��װMaven
��װ���֮����Ҫ����ͬ����setting�ļ�


## ��װNavicat for MySQL��
��װMysql���ݿ�ͻ������
���ݿ�������Ϣ��192.168.50.69:9980  yh_test/������ͬ��


## ��װGit �� SourceTree��
��װ`��Git-2.6.2-64-bit.exe��`����˾GIT��ַ��`git.dev.yoho.cn`����Ҫ�����˺����鳤����ĿȨ��
��װ��˾Ҫ���Git�ͻ������`��SourceTreeSetup_1.6.21��`
Ȼ������ҳGIT�л�ù��������Ŀ�����ӣ��磺`git@git.dev.yoho.cn:yoho30/yoho-core.git`����SourceTree�С���¡/�½�����Ӧ����Ŀ


## ��װJava IDE
��װ`��ideaIU-14.1.5.exe��`����װ���֮����ݸ��������һ����Ҫ�������²�����
`File-->Setting��Git��Maven(home,setting,repository)��KeyMap(��ݼ����ͬeclipse)��file encodings(IDE Encode,Default File Encoding)��Show line numbers`�ȵ�
��idea�е���Maven��Ŀ
����Service�����ã��������֮����Service������webӦ��
��˾����淶����`http://git.dev.yoho.cn/yoho-documents/api-interfaces/blob/master`


## �������ϵͳ
��˾�������ϵͳredmine��`http://redmine.yoho.cn`����Ҫ���鳤�Ӹ���Ŀ��������Ȩ��


## ������һ������Ϊ����������Ŀ����ԵĹ���
### ��������
���й��ܣ�App�ˡ�>�䡪>���䣺���������������10ƪ���¡�
���մ˹��ܣ�������һ����ֻ�ǽ���̨���ص����� ���� �ǳ����� ������������¡�

### �ҵ����й���App������URL��ַ��
��װfiddler4setup.exe����(���ڼ���ֻ�APPʵ�������URL������ͷ�����ؽ������Ϣ)��
`ע����������YOHOAPP��Android�汾��https����ģ�������URL����Ҫ���ֻ���yohoApp��app������Ա��ɲ��Ի����Ĳſ��Ե�`��װ��֮����Ҫ�������ã�
`Tools-->Fiddle Options-->Connections-->Allow remote computers to connect ������Fiddler listens on port:8888 ` 
`�ֻ����ӵ�wifi:YOHO-AP-->������������-->������:����ID-->�˿�8888`
�������ֻ�yohoApp�еĲ������Ϳ�����Fiddler�п��������������Ϣ��.

### ����App������URL��ַ���ҵ�gateway�ӿںͺ�̨����ӿ�
�ҵ�����Fiddler�м�¼�������URLΪ��`http://testservice.yoho.cn:28077/guang/api/v2/article/getList?app_version=4.1.0....��������`��gateway��Ŀ���ҵ���Ӧ��Controller��
���������ڴ�Controller���½�һ���ӿڡ�
����gateway��Controller�е��÷���service.call()�ĵ�ַCATEGORY_LOAD_URL = "guang.getList"�ҵ�������Ŀsns�е�restapi�ӿڣ����վͿ�����Service��Dao�ˣ�

### �������������±�ǩ��article_tags��Dao�ļ�(�п����е�������Ҫ)
���á�mybatis_generator�����ߣ��������ӵ����ݿ���Ϣ��������Ӧ���DO,IDAO,Mapper�ļ����޸Ĺ����С�generatorConfig.xml�����ɡ�
ע��Ҫ��������ɵ�IDao�ļ����޸ĳɷ��Ϲ�˾����淶���ļ�����IArticleDAO���������ļ��ŵ���Ӧ��Ŀ�ĺ���·����
PS�����Ҫ������DAO�Ľӿڣ��ӿ������в��ô�����������ID��ѯarticle��ֱ�ӱ�дselectById������selectArticleById����Ϊ��˾Ҫ��һ��DAOֱ�Ӳ���һ����
������ҵ�������дSNS��Ŀ�е�Service�ӿڣ�restApi�ӿڣ�gateway��Ŀ�еı�д���Ⱪ¶��Controller API.


## ���Ա���SNS��Ŀ
### ��װ������zookeeper
�ڱ��������а�װzookeeper��ֻ��Ҫ�����صİ�װ����ѹ������һ��Ĭ�ϵ������ļ�����(����.\conf\zoo_sample.cfg,����һ��zoo.cfg����)������zookeeper(.\bin\zkServer.cmd)

### ��װPostMan����
Postman��һ���ǿ�����ҳ�����뷢����ҳHTTP�����Chrome���������ģ������SNS������Ŀ��rest���� 
��Chrome�е�����Ӧ�õ���(��Ҫ�ȷ�ǽ)����postman������ɰ�װ��
ע��������һ���ӣ�һ��Ҫ��֤Chrome�İ汾�ǱȽϸߵ�(�������)����Ȼpostman��������֮�󣬲�������������

### ��ʼ����SNS��Ŀ
�ȱ�֤����������zookeeper�Ѿ�����
��IDEA�У��Ե��Եķ�ʽ����SNS��Ŀ�����ں��ʵĵط���ϵ�
�ٴ�postman����post��ʽ��������Body��ѡ�� raw  JSON(application/json)������JSON���������������������JSON��ʽ�Ĳ�����
Rest����һ��׼����ϣ��Ϳ��Ե������ߵ�Send��ť�ˡ�
���Ϳ��Կ�����IDEA�У��Ѿ����ֵ��Խ��棬��ҵ����Ĵ�����Լ��ɡ�
��������������±�д��IDAO��һ�������Ŀ����������Dao��ʱ�򣬻ᱨһ��`Table *** �����ڵĴ���`��
���ʱ����Ҫ��`yoho-sns\web\src\main\resources\databases.yml`��`yoho-sns\web\src\main\webapp\META-INF\autoconf\databases.yml`�ж�Ӧ�����ݿ��������϶�Ӧ��IDAO�ӿڣ��мǶ�Ҫ�޸ģ���Ȼ������DEV�Ȼ����У��͹��ˡ�
�����Ŀ�ܹ������ĵ�����ɣ��ӿڷ��ؽ������postman���ǿ��Կ������ؽ����
PS:����Լ����Ե���Ŀ�����õ�Maven���и��£���Ҫ����ȡRespository����IDEA�У���Ŀ�Ҽ�Maven����Reimport
PS:���ܸ�����Ŀ��zookeeper�����ò�һ������Ҫ��֤���ñ�����zookeeper�������ļ������޸���ȷ�� ����һ����`config.properties`�ļ��С�


## ���Ա���gateway��Ŀ
Gateway�е�Controller�ӿڱ�д���֮������gateway��Ŀ��
����postman(�����ֱ��URL����Ҳ����)����д��ȷ������URL(������)����get��ʽ�ύ���󣬲��۲췵�ؽ����
PS������һ���ӣ�����������ģ��app���͵����󣬲��ҿ���������أ��������󵽲���Controller����̨�������������лر�[500 ������֤����]���������������URL������debug��ʾ��&debug=XYZ��


## �ύ�Լ���д�Ĵ���
���������ӿ���֤���֮��ǧ��Ҫ�����ύ���뵽Git��ע�����ύ���أ������͵���������
ע���ύ֮ǰ����ȡ�����ⲻ�ɼ��ĳ�ͻ��
PS�����Ƽ��ܳ�ʱ��һ�����ύ̫��Ĵ��룬�������Լ��Ĵ���������������ʽ����ύ������ܶ���֮��һ�����ύ����Ĵ��룬�������Ծ���С�ı����ͻ�ķ�����Ҳ�������쵼�����Լ��Ĺ����������ǲ����ύ���������ܱ���Ĵ��롣


## ���չ淶Ҫ����yoho_Document���ύ�ӿڹ淶�ĵ���gateway�ӿ� 


## DEV��������
### �Լ�4030�����ύ�󣬺ϲ���֧�Ĺ��̣� 0430 �� �ϲ���dev
1����0430��֧����ȡһ��
2����dev��֧����ȡһ��
3��ȷ��Ŀǰ�򿪵ķ�֧��dev��֧���Ҽ�0430��֧���ϲ�����ǰ��֧��OK ��ɡ�
4����Ҫ����DEV��֧����Ҫ����DEV��֧��ɾ���ظ㣡����������

### ��0430�ϲ����DEV�ӿڷ�����205DEV��������
1��SSH��192.168.102.220���� dev/�������鳤
2������Ŀ¼�����г���  /home/dev/opps/deploy/   ./run.sh
   ���ݲ�ͬ��Ŀ�Ĵ��ţ����в�����£��磺15/1
   �п��ܻᷢ�����ɹ���ԭ���Ǳ���ʧ�ܣ�һ��ԭ����git�Զ������ļ����⡣�����ɹ�֮��205�����Ķ�Ӧ��Ŀ.ZIP�ļ������ڿ϶��Ǹ��µġ�
3������192.168.102.205���� root/�������鳤,���нӿڵ�ַ�ĸ���
   1��method��ʽ�� /home/nginx/conf/redirect.lua  ���������ӵĽӿ�
   2��URL��ʽ��    /home/nginx/conf/nginx.conf    ����û��ס������guang�Ľӿڶ��ǿ��Ե�
4������֮�󣬽���  /home/nginx/sbin/nginx -s reload,����û̫��

### ��֤205DEV���������Ľӿ�: DEV
1������gateway��method��ʽ����Ҫ����redirect.lua
   (�磺sns��ǰ����)http://devapi.yoho.cn:58078/?method=web.favorite.product&app_version=3.8.2&client_type=android&os_version=yohobuy%3Ah5&screen_size=720x1280&v=7&uid=3407014&limit=10&debug=XYZ
1������gateway����method��ʽ����Ҫ����nginx
   (�磺sns�¿�������)http://devservice.yoho.cn:58077/guang/api/v2/article/getStarClassroomArticleList?app_version=4.1.0&page=1&debug=XYZ
   (�磺sns��ǰ����)http://devservice.yoho.cn:58077/guang/api/v2/article/getList?app_version=4.1.0.1603120001&client_secret=0955a436492e71767d4b9a825a367799&client_type=iphone&gender=2%2C3&limit=20&os_version=9.2.1&page=1&screen_size=320x568&sort_id=0&udid=258e608815a77ba2f9094da936f896ef4fd65721&uid=8039983&v=7&yh_channel=2
   (�磺resource����)http://devservice.yoho.cn:58077/operations/api/v5/resource/get?content_code=b38b9c4f1c76f89533e9214629b458e4&debug=XYZ
2�����ڵ�ַ
   (Get��ʽ)http://192.168.102.205:8080/gateway/guang/api/v2/article/getList?app_version=4.1.0.1603120001&client_secret=0955a436492e71767d4b9a825a367799&client_type=iphone&gender=2%2C3&limit=20&os_version=9.2.1&page=1&screen_size=320x568&sort_id=0&udid=258e608815a77ba2f9094da936f896ef4fd65721&uid=8039983&v=7&yh_channel=2
   (Post��ʽ)http://192.168.102.205:8082/sns/ArticleRest/getList

### ����������⣬ͨ�����·�ʽ�Ų�
1����JAR���е��ļ��Ƿ����µģ�
   /home/dev/yoho-gateway/yoho-gateway.zip(./deploy�ǽ�ѹ֮���Ŀ¼) �� /home/dev/yoho-gateway/deploy/gateway-war/WEB-INF/lib/yoho-gateway-service-1.0.0-SNAPSHOT.jar
   ��class�ļ����Ƿ��ʵ�ʰ汾һ���ġ�
2���鿴��־����������
   /Data/logs/gateway/warn-log.log  ��  /Data/logs/sns/debug.log��warn.log
   PS��logger.debug���ֲ���ӡ��־��logger.info�Ǵ�ӡ��־�ģ�������־�ڡ�debug-log.log���ļ��У�������쳣�����ڡ�warn.log���ļ��С�


## �ҶȻ�������֤�ӿ�(��δ��ʼ������ɺ�������� ) 

