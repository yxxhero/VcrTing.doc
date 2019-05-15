### 安装 Anaconda3
1. 先需安装python3 环境：
`
yum install -y zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gcc make
`
`
yum install openssl-devel   -y

yum install zlib-devel  -y
`

2. 下载Anaconda 
   wget https://repo.anaconda.com/archive/Anaconda3-2019.03-Linux-x86_64.sh

3. 安装Anaconda 
   bash Anaconda*.sh

4. 配置Anaconda 环境变量 
    vi ~/.bashrc 
    i 
    export PATH=~/anaconda3/bin:$PATH 
    esc :wq 
    source ~/.bashrc

5. 虚拟环境
    conda create --name web
    source activate web
    source deactivate
    conda remove -n web --all

6. 安装Python 包
   导出 pip freeze > setup.txt 
   安装 pip install -r setup.txt

### 使用Nginx