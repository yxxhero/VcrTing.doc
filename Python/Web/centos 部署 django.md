### 安装 Anaconda3
1. 先需安装python 安装依赖：  
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

### 部署前的准备工作  
1. uwsgi   
    确保有了uwsgi 包  
    写uwsgi.ini 到项目根目录   
    内容:  
    `[uwsgi]`  
    `socket = 127.0.0.1:8001`  
    `chdir = /root/Project_root`  
    `module = Project_root.wsgi`  
    `master = true`  
    `processes = 2`  
    `vacuum = true` 
    前排提示，当前不推荐使用uwsgi 部署，它总是出现问题！！我们推荐使用gunicorn 

2. 安装Nginx   
    `yum -y install nginx`  
    `测试一下: nginx -t`  

3. 配置Nginx  
    配置文件在 /etc/nginx/nginx.conf  

### 实用命令  
1. centos 根据 port 杀死进程    
    `查找进程ip: lsof -i :80`  
    `击杀ip进程: kill -9 ip`  

### 安排Django  
1. 静态文件  
    确保static 路径的正确性  
    django 采集静态资源：`python3 manage.py collectionstatic`  
    确保django Allow_hosts 参数正确，前期可以设为 ['*']  
    
2. 尝试启动  
   使用django原有方式启动：`python3 manage.py runserver 0.0.0.0:80`  
   在外网访问你的域名试试先。  

3. 设置为gunicorn启动  
   