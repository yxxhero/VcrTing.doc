### 安装 Anaconda3
1. 先需安装python3 环境：
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
   `导出 pip freeze > setup.txt `
   `安装 pip install -r setup.txt`

### 获取项目
1. 下载项目 
   `wget https://github.com/VcrTing/WX_DEMO/archive/master.zip`

2. 解压项目 
   `yum install zip unzip`

   `压缩: zip -r master.zip ./`
   `解压: unzip master.zip -d ./`

### 部署
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

2. 安装Nginx 
   `yum -y install nginx`
   `测试一下 nginx -t`

3. 配置Nginx
   配置文件在 /etc/nginx/nginx.conf