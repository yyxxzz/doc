# Nginx入门&配置
## windows安装
官网下载nginx-1.8.1.zip，解压
## 启动Nginx
默认配置在conf/nginx.conf
``` 
启动 nginx.exe
``` 
浏览器访问localhost:80/  成功显示 Welcome to nginx! 

### 常用命令
| 名称 | 命令 | 
| :-- | :--  | 
| 关闭 | nginx -s stop |
| 重启 | nginx -s reopen |
| 重新load | nginx -s reload |
 
 
## 配置测试APP 访问dev环境
``` 
1.配置Fiddler与手机网络代理，让手机通过自己机器间接访问服务
``` 
2.配置hosts   添加域名 127.0.0.1 testapi.yoho.cn
``` 
3.配置nginx(nginx.conf)   添加server节点：
```json
 server
	{
		#testapi的默认访问端口
		listen 28078;
		server_name localhost;
		location / {
			proxy_redirect off;
			proxy_set_header Host $host;
			proxy_set_header X-Real-IP $remote_addr;
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
			#devapi访问路径
			proxy_pass http://devapi.yoho.cn:58078;     
		}
		#访问路径
		access_log logs/dev_access.log;
	}
```  
4.启动nginx，手机app访问转到dev环境