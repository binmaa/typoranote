[TOC]

#### 1. ZIP解压

```shell
#如果不是用括号里面的内容，则解压文件在当前工作目录
unzip [参数] [压缩文件]  （-d [目录]）  
-o:不提示的情况下覆盖文件;
-d:-d /home/sunny指明将文件解压缩到/home/sunny目录下。

unzip -o -d /home/sunny myfile.zip
```

#### 2. ZIP压缩

```shell
#将当前目录下的所有文件和文件夹全部压缩成myfile.zip内联代码块文件,内联代码块-r表示递归压缩子目录下所有文件
zip -r myfile.zip ./*

#向压缩文件中myfile.zip中添加rpm_info.txt文件。
zip -m myfile.zip ./rpm_info.txt

##删除压缩文件中smart.txt文件
zip -d myfile.zip smart.txt
```

#### 3. 端口占用

```shell
netstat -anp | grep 8060
kill -s 9 9646(进程号)
```

#### 4. 环境变量

```shell
vim /etc/profile
#添加
export JAVA_HOME=/usr/local/jdk1.8.0_221 
export PATH=$JAVA_HOME/bin:$PATH 
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tool.jar

#加载
source /etc/profile
```

#### 5. 本地rpm安装

```shell
#安装
rpm -i example.rpm 安装 example.rpm 包；
rpm -iv example.rpm 安装 example.rpm 包并在安装过程中显示正在安装的文件信息；
rpm -ivh example.rpm 安装 example.rpm 包并在安装过程中显示正在安装的文件信息及安装进度

rpm -ivh InforSuiteAS-10-0.x86_64

#卸载
rpm -e example

#升级
rpm -Uvh example.rpm

#查找安装位置
1. rpm -qa |grep InforSuiteAS
 #得到具体名称InforSuiteAS-10-0.x86_64
2. rpm -ql InforSuiteAS-10-0.x86_64
 #查位置  
```

#### 6. 实时监控（日志）

```shell
tailf server.log
tail -f server.log
```

