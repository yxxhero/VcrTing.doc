<center><img width = '170' src ="https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=1967034059,4011926928&fm=11&gp=0.jpg"/></center>  
  
### 安装 Anaconda3
0. 先需安装python 安装依赖：  
    `yum install -y zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gcc make`  
    `yum install openssl-devel   -y`  
    `yum install zlib-devel  -y`  

1. 下载Anaconda   
   `wget https://repo.anaconda.com/archive/Anaconda3-2019.03-Linux-x86_64.sh`  

2. 安装Anaconda  
   `bash Anaconda*.sh`  
   一直 yes / enter

3. 配置Anaconda 环境变量   
    `vi ~/.bashrc`  
    `i`   
    `export PATH=/root/anaconda3/bin:$PATH `  
    `esc :wq `  
    `source ~/.bashrc`  
    `source /etc/profile`  

4. 虚拟环境  
    `conda create --name web`  
    `source activate web`  
    `source deactivate`  
    `conda remove -n web --all`  

5. 安装Python 包  
   `导出: pip freeze > setup.txt `  
   `安装: pip install -r setup.txt`  

### 获取项目  
1. 下载项目   
   `wget https://github.com/VcrTing/WX_DEMO/archive/master.zip`  

2. 解压项目   
   `yum install zip unzip`  

   `压缩: zip -r master.zip ./`  
   `解压: unzip master.zip -d ./`  
   我把项目放在/root/下面，项目名 WXBK  

### 实用命令  
1. centos 根据 port 杀死进程    
    `查找进程ip: lsof -i :80`  
    `击杀ip进程: kill -9 ip`  

### 安排Django  
1. 静态文件  
    确保static 路径的正确性  
    django 采集静态资源：`python3 manage.py collectstatic`  
    确保django Allow_hosts 参数正确，前期可以设为 ['*']  
    
2. 尝试启动  
    使用django原有方式启动：`python3 manage.py runserver 0.0.0.0:80`  
    在外网访问你的域名试试先。  

3. 设置为gunicorn启动  
    安装gunicorn  
    `pip install gunicorn` 
    然后setting.py 中注册gunicorn 这个app   
    在项目根目录下书写配置文件gunicorn.conf.py，让gunicorn 承载django   
    `import logging`  
    `import logging.handlers`  
    `from logging.handlers import WatchedFileHandler`  
    `import os`  
    `import multiprocessing`  
  
    `bind = "0.0.0.0:8091"`  
    `backlog = 512 #监听队列数量，64-2048`  
    `worker_class = 'sync' # 使用gevent模式，还可以使用sync 模式，默认的是sync模式`  
    `workers = multiprocessing.cpu_count()`  
    `threads = multiprocessing.cpu_count()*4`  
    `loglevel = 'error' # error / info`  
    `access_log_format = '%(t)s %(p)s %(h)s "%(r)s" %(s)s %(L)s %(b)s %(f)s" "%(a)s"'`  
  
    `accesslog = "/root/Project/gunicorn_access.log"`  
    `errorlog = "/root/Project/home/log/gunicorn_error.log" # "路径" 表示日志写入文件，"-" 表示在控制台进行标准输出`  
  
    `proc_name = 'Project' # 进程名`   
    
4. gunicorn启动  
    在项目根目录下cmd，输入命令  
    `gunicorn -c gunicorn.conf.py Project.wsgi:application`   
    在外网访问你的域名试试先，要注意gunicorn中配置的端口号  

### 安排Nginx  
1. 安装Nginx   
    `yum -y install nginx`  
    `测试一下: nginx -t`  

2. 保证Nginx 可用性  
    输入  
    `nginx -s reload`
    看看有什么问题，如果 invalid PID number "" 的话，输入  
    `nginx -c /etc/nginx/nginx.conf`  
    来排除错误，然后再reload 一下，  
    尝试外网访问你的域名。  
    注意几个问题：80端口不能被非nginx占用，Nginx配置里面只允许出现一个80监听的server{}。  

3. 书写配置文件  
    只需要把server {} 复制到 /etc/nginx/nginx.conf 文件的 http {} 括号里面就行了，但是要注意80 端口的server{} 只能有一个 

    `server {`  
    `    listen 80;`  
    `    server_name  backend.svr.szmentor.ltd;`  
    `    charset utf-8;`  
    `    client_max_body_size 200m;`  
    `    access_log  /root/WXBK/nginx-access.log;`  
    `    error_log  /root/WXBK/nginx-err.log;`  
    `    location = /favicon.ico { `  
    `        access_log off;`  
    `        log_not_found off; `  
    `    }`  
    `    location /static/ {`  
    `        #静态文件如js，css的存放目录`  
    `        alias /root/WXBk/Static/;`  
    `    }`  
    `    location / {`  
    `        proxy_pass http://0.0.0.0:8001; # gunicorn 端口`  
    `        proxy_redirect     off;`  
    `        proxy_set_header   Host                 $http_host;`  
    `        proxy_set_header   X-Real-IP            $remote_addr;`  
    `        proxy_set_header   X-Forwarded-For      $proxy_add_x_forwarded_for;`  
    `        proxy_set_header   X-Forwarded-Proto    $scheme;`  
    `    }`  
    `}`  

4. 外网访问  
    保证 gunicorn 开启，保证nginx在监听  
    访问外网  
     
    若静态资源 403 错误，需将 nginx.conf 
    nginx.conf 头部的 user nginx; 改为 user root;  
    
    若静态资源 404 错误，Nginx 有错误日志，你去仔细检查错误日志鸭～  

### 完成啦！！！
<br/>
<p align='right'>💗</p>
