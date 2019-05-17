<center><img width = '170' src ="https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=1967034059,4011926928&fm=11&gp=0.jpg"/></center>  

### SSL认证  
1. 环境  
   确保外网可访问你的Nginx  
   安装一些东西：  
   `yum install epel-release`  
   `yum install git`  

2. Letsencrypt  
   `cd /opt`  
   `git clone https://github.com/letsencrypt/letsencrypt`  

3. Stop Nginx  
    必须使的Nginx 停止服务  
    `service nginx stop`  
    `systemctl stop nginx`  
    `ss -tln`  

4. 获取SSL 认证信息  
   `cd /opt/letsencrypt`  
   `./letsencrypt-auto certonly --standalone -d Your_domain`  
   这里是没有子域名的，如果有子域名，得去百度一下有子域名怎么弄！  
   然后根据提示输入一些信息。  
   见到Congratulations 就说明获取成功啦！！！  
   然后看到有 /opt/letsencrypt/live/ 就是包含/live/的文件夹
   这就是成功的  不过，live 文件夹下面的所有文件的权限要注意一下。  

5. 结合 Nginx  
    `/opt/letsencrypt/bin/pip install -U letsencrypt-nginx`  
    `/letsencrypt-auto --nginx -d Your_domin -m Email`  
    `mkdir /etc/nginx/ssl-certs/`  
    `openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/ssl-certs/nginx.key -out /etc/nginx/ssl-certs/nginx.crt`  
    根据提示输入一下认证信息  
    
6. 配置SSL  
    `vi /etc/nginx/conf.d/Your.conf`  
    在server {} 中添加:  

    `listen 443 ssl;`  
    `listen [::]:443 ssl;`  
    
    `ssl on;`  
    `ssl_certificate /etc/nginx/ssl-certs/nginx.crt;`  
    `ssl_trusted_certificate /etc/nginx/ssl-certs/nginx.crt;`  
    `ssl_certificate_key /etc/nginx/ssl-certs/nginx.key;`  

7. 重启Nginx  
   `systemctl start nginx`  
   `nginx -s reload`  
   外网尝试使用 https 访问  

