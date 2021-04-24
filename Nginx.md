[TOC]

**Nginx是一款自由的、开源的、高性能的HTTP服务器和反向代理服务器；同时也是一个IMAP、POP3、SMTP代理服务器；Nginx可以作为一个HTTP服务器进行网站的发布处理，另外Nginx可以作为反向代理进行负载均衡的实现。**

<https://blog.csdn.net/diwudong1059/article/details/102072095>



#####  1. 代理

所谓代理就是一个代表、一个渠道；

此时就涉及到两个角色，一个是被代理角色，一个是目标角色，被代理角色通过这个代理访问目标角色完成一些任务的过程称为代理操作过程；如同生活中的专卖店~客人到adidas专卖店买了一双鞋，这个专卖店就是代理，被代理角色就是adidas厂家，目标角色就是用户。

1.  正向代理

**正向代理，"它代理的是客户端，代客户端发出请求"**

正向代理最大的特点是客户端非常明确要访问的服务器地址；服务器只清楚请求来自哪个代理服务器，而不清楚来自哪个具体的客户端；正向代理模式屏蔽或者隐藏了真实客户端信息。（例如翻墙）

正向代理，"它代理的是客户端，代客户端发出请求"，是一个位于客户端和原始服务器(origin server)之间的服务器，为了从原始服务器取得内容，客户端向代理发送一个请求并指定目标(原始服务器)，然后代理向原始服务器转交请求并将获得的内容返回给客户端。客户端必须要进行一些特别的设置才能使用正向代理。

正向代理的用途：
（1）访问原来无法访问的资源，如Google 、VPN
（2） 可以做缓存，加速访问资源
（3）对客户端访问授权，上网进行认证
（4）代理可以记录用户访问记录（上网行为管理），对外隐藏用户信息

2. 反向代理

　**反向代理，"它代理的是服务端，代服务端接收请求"，**主要用于服务器集群分布式部署的情况下，**反向代理隐藏了服务器的信息。**

![img](image\反向代理.png)

通常情况下，我们在实际项目操作时，正向代理和反向代理很有可能会存在在一个应用场景中，正向代理代理客户端的请求去访问目标服务器，目标服务器是一个反向单利服务器，反向代理了多台真实的业务处理服务器

3. Nginx 负载均衡调度算法

   + weight轮询（默认，常用，具有HA功效！）：接收到的请求按照权重分配到不同的后端服务器，即使在使用过程中，某一台后端服务器宕机，Nginx会自动将该服务器剔除出队列，请求受理情况不会受到任何影响。 这种方式下，可以给不同的后端服务器设置一个权重值(weight)，用于调整不同的服务器上请求的分配率；权重数据越大，被分配到请求的几率越大；该权重值，主要是针对实际工作环境中不同的后端服务器硬件配置进行调整的。

   + ip_hash（常用）：每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，这也在一定程度上解决了集群部署环境下session共享的问题。

   + fair：智能调整调度算法，动态的根据后端服务器的请求处理到响应的时间进行均衡分配，响应时间短处理效率高的服务器分配到请求的概率高，响应时间长处理效率低的服务器分配到的请求少；结合了前两者的优点的一种调度算法。但是需要注意的是Nginx默认不支持fair算法，如果要使用这种调度算法，请安装upstream_fair模块。

   + url_hash：按照访问的url的hash结果分配请求，每个请求的url会指向后端固定的某个服务器，可以在Nginx作为静态服务器的情况下提高缓存效率。同样要注意Nginx默认不支持这种调度算法，要使用的话需要安装Nginx的hash软件包。

##### 2.常用命令

```shell
#启动
./nginx 
#检查 nginx.conf配置文件
./nginx -t
#重启
./nginx -s reload
#停止
#出服务：
./nginx -s quit
#强制关闭服务：
./nginx -s stop
#使用配置文件：
./nginx -c "配置文件路径"
#使用帮助：
./nginx -h
```

##### 3.Nginx配置文件

用最多的三个核心功能是静态服务器、反向代理和负载均衡

详细见附录

##### 4. Nginx主要配置

###### 1.静态http服务器配置

> Nginx是一个HTTP服务器，可以将服务器上的静态文件（如HTML、图片）通过HTTP协议展现给客户端。

```nginx
server {
    listen 80; 　　# 端口
    server_name www.binmaa.com; 　　# 域名   多个server时不同的域名访问不同的页面
    location / {　　　　　　　　　　　　　# 代表这是项目根目录
        root C:\work\design; 　 # 虚拟目录
        index images\二维码.gif;
    }
}
server {
    listen 80; 　　# 端口
    server_name www.binmaa2.com; 　　# 域名   多个server时不同的域名访问不同的页面
    location / {　　　　　　　　　　　　　# 代表这是项目根目录
        root C:\work\design; 　 # 虚拟目录
        index  index.html index.htm;
    }
}
```

hosts文件

```shell
127.0.0.1 www.binmaa.com
127.0.0.1 www.binmaa1.com
```

http://www.binmaa1.com http://www.binmaa1.com 访问不同域名访问不同页面

请求时请求标头：

```html
GET / HTTP/1.1
Host: www.binmaa.com:8888
Connection: keep-alive
Cache-Control: max-age=0
```

###### 2. 反向代理服务器配置

```nginx
server {
		#侦听8888端口
        listen 80;
		#定义使用 www.bluewave.com访问域名
        server_name  www.bluewave.com;
		# 定位，把特殊的路径或文件再次定位。
        location / {
            #root   C:\work\design;
            #index  index.html index.htm;
			proxy_pass http://127.0.0.1:8082; #应用服务器HTTP地址
        }
}
```

访问http://www.bluewave.com/hz7就可以了

另：[NGINX服务器的反向代理PROXY_PASS配置方法讲解](https://www.cnblogs.com/lianxuan1768/p/8383804.html)

###### 3.负载均衡（默认轮询）

负载均衡一种策略，用于防止一台服务器过载，而其他服务器闲置情况发生的策略。通过该策略可以使得提供相同服务的服务器负载基本相同。说得直白一点，就是当客户端发起一个请求之后，负载均衡会通过预先设定好的策略将该请求转发给上游的一台服务器进行处理。

```nginx
upstream hz7 {
    ip_hash; #固定访客
    server 192.168.200.160:8082 weight=1;# 应用服务器1
    server 192.168.200.160:8080 weight=10 down;# 应用服务器2
}
server {
    listen       80;
    server_name  http://192.168.200.102;
    location / {
        # root   html;
        # index  index.html index.htm;
        proxy_pass http://hz7;
    }
}
```

> 1. down: 当前服务器暂不参与负载
> 2. weight: 权重，值越大，服务器的负载量越大。
> 3. max_fails：允许请求失败的次数，默认为1。
> 4. fail_timeout:max_fails次失败后暂停的时间。 例如：fail_timeout=60s
> 5. backup：备份机， 只有其它所有的非backup机器down或者忙时才会请求backup机器。

**两种常见的负载均衡hash策略，以及对应的使用场景。**

1. ip_hash（通过客户端请求ip进行hash，再通过hash值选择后端server）

+ 当你服务端的一个特定url路径会被同一个用户连续访问时，如果负载均衡策略还是轮询的话，那该用户的多次访问会被打到各台服务器上，这显然并不高效（会建立多次http链接等问题）。甚至考虑一种极端情况，用户需要分片上传文件到服务器下，然后再由服务器将分片合并，这时如果用户的请求到达了不同的服务器，那么分片将存储于不同的服务器目录中，导致无法将分片合并。所以，此类场景可以考虑采用nginx提供的ip_hash策略。既能满足每个用户请求到同一台服务器，又能满足不同用户之间负载均衡。

+ 配置

  ```nginx
  upstream hz7 {
      ip_hash; #固定访客
      server 192.168.200.160:8082 weight=1;# 应用服务器1
      server 192.168.200.160:8080 weight=10 down;# 应用服务器2
  }
  server {
      listen       80;
      server_name  http://192.168.200.102;
      location / {
          # root   html;
          # index  index.html index.htm;
          proxy_pass http://hz7;
      }
  }
  ```

2. url_hash（通过请求url进行hash，再通过hash值选择后端server）

+ 一般来讲，要用到urlhash，是要配合缓存命中来使用。举一个我遇到的实例：有一个服务器集群A，需要对外提供文件下载，由于文件上传量巨大，没法存储到服务器磁盘中，所以用到了第三方云存储来做文件存储。服务器集群A收到客户端请求之后，需要从云存储中下载文件然后返回，为了省去不必要的网络带宽和下载耗时，在服务器集群A上做了一层临时缓存（缓存一个月）。由于是服务器集群，所以同一个资源多次请求，可能会到达不同的服务器上，导致不必要的多次下载，缓存命中率不高，以及一些资源时间的浪费。在此类场景下，为了使得缓存命中率提高，很适合使用url_hash策略，同一个url（也就是同一个资源请求）会到达同一台机器，一旦缓存住了资源，再此收到请求，就可以从缓存中读取，既减少了带宽，也减少的下载时间。
+ 配置

```nginx
upstream somestream {
    hash $request_uri consistent; #consistent 使用一致性哈希
    server 192.168.244.1:8080;
    server 192.168.244.2:8080;
    server 192.168.244.3:8080;
    server 192.168.244.4:8080;
 
}
server {
    listen 8081 default;
    server_name test.csdn.net;
    charset utf-8;
    location /get {
    proxy_pass http://somestream;
    }  
}
```

上述同样也是一个极简的监听8081端口的nginx服务，当请求url为/get时，会走url_hash；同样配置了upstream模块，hash $request_uri表明了是按照url规则进行hash策略。



https://daguang.blog.csdn.net/article/details/104750514

**Nginx容灾 搭建Keepalived**	

[https://www.cnblogs.com/wang-meng/p/5861174.html](https://www.cnblogs.com/wang-meng/p/5861174.html)

> 首先Keepalived可以在主机上产生一个虚拟的ip, 这里叫做vip(v是virtual的意思):192.168.200.150, keepalived会将这个vip绑定到交换机上.
> 当用户访问主机:192.168.200.129时, 交换机会通过这个ip和vip的对应找到192.168.200.129上的Nginx进行处理.
> 如果当有一天192.168.200.129上的Nginx挂掉的时候, Keepalived会立即在备机上生成一个相同的vip: 192.168.200.150, 当用户继续访问192.168.200.129时, 交换机上已经绑定了vip, 这时发现这个vip是存在于192.168.200.130上面的, 所以直接将请求转发到了备机上. 
> 如果主机被修复好能够继续对外提供服务时, 这时keepalived会将主机上继续生成这个vip, 同时回收在备机上生成的vip. 这个是通过心跳检查来判断主机已恢复使用.

###### 4. 虚拟主机配置

> 有的网站访问量大，需要负载均衡。然而并不是所有网站都如此出色，有的网站，由于访问量太小，需要节省成本，将多个网站部署在同一台服务器上。例如将www.aaa.com和www.bbb.com两个网站部署在同一台服务器上，两个域名解析到同一个IP地址，但是用户通过两个域名却可以打开两个完全不同的网站，互相不影响，就像访问两个服务器一样，所以叫两个虚拟主机。

```nginx
server {
    listen 80;
    server_name www.aaa.com; 　　# www.aaa.com域名
    location / {
        proxy_pass http://localhost:8080; 　　# 对应端口号8080
    }
}
server {
    listen 80;
    server_name www.bbb.com; 　　# www.bbb.com域名
    location / {
        proxy_pass http://localhost:8081; 　　# 对应端口号8081
    }
}
```

> 在服务器8080和8081分别开了一个应用，客户端通过不同的域名访问，根据server_name可以反向代理到对应的应用服务器。
>
> 虚拟主机的原理是通过HTTP请求头中的`Host`是否匹配s`erver_name`来实现的，有兴趣的同学可以研究一下HTTP协议。

###### 5.location详解

> 语法规则： `location [=|~|~*|^~] /uri/ { … }`
>
> - `=` 开头表示精确匹配
>
> - `^~` 开头表示uri以某个常规字符串开头，理解为匹配 url路径即可。nginx不对url做编码，因此请求为/static/20%/aa，可以被规则^~ /static/ /aa匹配到（注意是空格）。以xx开头
>
> - `~` 开头表示区分大小写的正则匹配           以xx结尾
>
> - `~*` 开头表示不区分大小写的正则匹配        以xx结尾
>
> - `!~`和`!~*`分别为区分大小写不匹配及不区分大小写不匹配 的正则
>
> - `/` 通用匹配，任何请求都会匹配到。
>
>   
>
> - 首先精确匹配 =-》其次以xx开头匹配^~-》然后是按文件中顺序的正则匹配-》最后是交给 / 通用匹配。 当有匹配成功时候，停止匹配，按当前匹配规则处理请求。

```nginx
location = / {
# 只匹配 / 查询。
}
location / {
# 匹配任何查询，因为所有请求都已 / 开头。但是正则表达式规则和长的块规则将被优先和查询匹配。
}
location ^~ /p_w_picpaths/ {
# 匹配任何已 /p_w_picpaths/ 开头的任何查询并且停止搜索。任何正则表达式将不会被测试。
}
location ~*.(gif|jpg|jpeg)$ {
# 匹配任何已 gif、jpg 或 jpeg 结尾的请求。
}
```

**动静分离例子 及防盗链**

```nginx
	server {
		listen 80;
		server_name www.binmaa1.com;
		
		location ~*.(gif|jpg|jpeg|png)$ {
			root /Softwave/tomcat/apache-tomcat8-8888/webapps/;
			
			#添加防盗链
			valid_referers  www.binmaa1.com 192.168.200.73; #允许访问该目录的域名或IP
			if ($invalid_referer){
				rewrite ^/ https://dss0.bdstatic.com/5aV1bjqh_Q23odCf/static/superman/img/logo/bd_logo1-66368c33f8.png;
			}
		}
		location / {
			proxy_pass http://192.168.200.73:8080;
		}
	}
```



##### 附1：基础配置

```nginx
main                                # 全局配置
events {                            # 工作模式配置
}

http {                              # http设置
    ....
    server {                        # 服务器主机配置（虚拟主机、反向代理等）
        ....
        location {                  # 路由配置（虚拟目录等）
            ....
        }
        location path {
            ....
        }
        location otherpath {
            ....
        }
    }
    server {
        ....
        location {
            ....
        }
    }
    upstream name {                  # 负载均衡配置
        ....
    }
}
```

###### 1.main模块

> - user  用来指定nginx worker进程运行用户以及用户组，默认nobody账号运行
> - worker_processes  指定nginx要开启的子进程数量，运行过程中监控每个进程消耗内存(一般几M~几十M不等)根据实际情况进行调整，通常数量是CPU内核数量的整数倍
> - error_log  定义错误日志文件的位置及输出级别【debug / info / notice / warn / error / crit】
> - pid  用来指定进程id的存储文件的位置
> - worker_rlimit_nofile  用于指定一个进程可以打开最多文件数量的描述
> - ...

###### 2.event模块

>- worker_connections  指定最大可以同时接收的连接数量，这里一定要注意，最大连接数量是和worker processes共同决定的。
>- multi_accept  配置指定nginx在收到一个新连接通知后尽可能多的接受更多的连接
>- use epoll  配置指定了线程轮询的方法，如果是linux2.6+，使用epoll，如果是BSD如Mac请使用Kqueue
>- ...

###### 3.http模块

> 作为web服务器，http模块是nginx最核心的一个模块，配置项也是比较多的，项目中会设置到很多的实际业务场景，需要根据硬件信息进行适当的配置。

1. 基础配置

   ```nginx
   sendfile on：配置on让sendfile发挥作用，将文件的回写过程交给数据缓冲去去完成，而不是放在应用中完成，这样的话在性能提升有有好处
   tcp_nopush on：让nginx在一个数据包中发送所有的头文件，而不是一个一个单独发
   tcp_nodelay on：让nginx不要缓存数据，而是一段一段发送，如果数据的传输有实时性的要求的话可以配置它，发送完一小段数据就立刻能得到返回值，但是不要滥用哦
   
   keepalive_timeout 10：给客户端分配连接超时时间，服务器会在这个时间过后关闭连接。一般设置时间较短，可以让nginx工作持续性更好
   client_header_timeout 10：设置请求头的超时时间
   client_body_timeout 10:设置请求体的超时时间
   send_timeout 10：指定客户端响应超时时间，如果客户端两次操作间隔超过这个时间，服务器就会关闭这个链接
   
   limit_conn_zone $binary_remote_addr zone=addr:5m ：设置用于保存各种key的共享内存的参数，
   limit_conn addr 100: 给定的key设置最大连接数
   
   server_tokens：虽然不会让nginx执行速度更快，但是可以在错误页面关闭nginx版本提示，对于网站安全性的提升有好处哦
   include /etc/nginx/mime.types：指定在当前文件中包含另一个文件的指令
   default_type application/octet-stream：指定默认处理的文件类型可以是二进制
   type_hash_max_size 2048：混淆数据，影响三列冲突率，值越大消耗内存越多，散列key冲突率会降低，检索速度更快；值越小key，占用内存较少，冲突率越高，检索速度变慢
   ```

2. 日志配置

   ```nginx
   access_log logs/access.log：设置存储访问记录的日志
   error_log logs/error.log：设置存储记录错误发生的日志
   ```

3. SSL证书配置

   ```nginx
   ssl_protocols：指令用于启动特定的加密协议，nginx在1.1.13和1.0.12版本后默认是ssl_protocols SSLv3 TLSv1 TLSv1.1 TLSv1.2，TLSv1.1与TLSv1.2要确保OpenSSL >= 1.0.1 ，SSLv3 现在还有很多地方在用但有不少被攻击的漏洞。
   ssl prefer server ciphers：设置协商加密算法时，优先使用我们服务端的加密套件，而不是客户端浏览器的加密套件
   ```

4. 压缩配置

   ```nginx
   gzip 是告诉nginx采用gzip压缩的形式发送数据。这将会减少我们发送的数据量。
   gzip_disable 为指定的客户端禁用gzip功能。我们设置成IE6或者更低版本以使我们的方案能够广泛兼容。
   gzip_static 告诉nginx在压缩资源之前，先查找是否有预先gzip处理过的资源。这要求你预先压缩你的文件（在这个例子中被注释掉了），从而允许你使用最高压缩比，这样nginx就不用再压缩这些文件了（想要更详尽的gzip_static的信息，请点击这里）。
   gzip_proxied 允许或者禁止压缩基于请求和响应的响应流。我们设置为any，意味着将会压缩所有的请求。
   gzip_min_length 设置对数据启用压缩的最少字节数。如果一个请求小于1000字节，我们最好不要压缩它，因为压缩这些小的数据会降低处理此请求的所有进程的速度。
   gzip_comp_level 设置数据的压缩等级。这个等级可以是1-9之间的任意数值，9是最慢但是压缩比最大的。我们设置为4，这是一个比较折中的设置。
   gzip_type 设置需要压缩的数据格式。上面例子中已经有一些了，你也可以再添加更多的格式。
   ```

5. 文件缓存配置

   ```nginx
   open_file_cache 打开缓存的同时也指定了缓存最大数目，以及缓存的时间。我们可以设置一个相对高的最大时间，这样我们可以在它们不活动超过20秒后清除掉。
   open_file_cache_valid 在open_file_cache中指定检测正确信息的间隔时间。
   open_file_cache_min_uses 定义了open_file_cache中指令参数不活动时间期间里最小的文件数。
   open_file_cache_errors 指定了当搜索一个文件时是否缓存错误信息，也包括再次给配置中添加文件。我们也包括了服务器模块，这些是在不同文件中定义的。如果你的服务器模块不在这些位置，你就得修改这一行来指定正确的位置。
   ```

###### 4.server模块

> server模块配置是http模块中的一个子模块，用来定义一个虚拟访问主机，也就是一个虚拟服务器的配置信息。
>
> - server：一个虚拟主机的配置，一个http中可以配置多个server
> - server_name：用来指定ip地址或者域名，多个配置之间用空格分隔
> - charset：用于设置www/路径中配置的网页的默认编码格式
> - access_log：用于指定该虚拟主机服务器中的访问记录日志存放路径
> - error_log：用于指定该虚拟主机服务器中访问错误日志的存放路径

```nginx
server {
    listen         80;
    server_name    localhost  192.168.1.100;
    charset        utf-8;
    access_log     logs/access.log;
    error_log      logs/error.log;
    ......
}
```

###### 5.location模块

> location模块是Nginx配置中出现最多的一个配置，主要用于配置路由访问信息。
>
> 在路由访问信息配置中关联到反向代理、负载均衡等等各项功能

1. 基本配置

   ```nginx
   location / {
       root    /nginx/www;
       index    index.php index.html index.htm;
   }
   ```

   > - location /：表示匹配访问根目录
   > - root：用于指定访问根目录时，访问虚拟主机的web目录
   > - index：在不指定访问具体资源时，默认展示的资源文件列表

2. 反向代理配置

   > 通过反向代理代理服务器访问模式，通过proxy_set配置让客户端访问透明化。

   ```nginx
   location / {
       proxy_pass http://localhost:8888;
       proxy_set_header X-real-ip $remote_addr;
       proxy_set_header Host $http_host;
   }
   ```

3. uwsgi配置

   ```nginx
   location / {
       include uwsgi_params;
       uwsgi_pass localhost:8888;
   }
   ```

   

###### 6.负载均衡配置

> upstream模块主要负责负载均衡的配置，通过默认的轮询调度方式来分发请求到后端服务器。简单的配置方式如下。

```nginx
upstream name {
    ip_hash;
    server 192.168.1.100:8000 weight=9;
    server 192.168.1.100:8001 down;
    server 192.168.1.100:8002 max_fails=3;
    server 192.168.1.100:8003 fail_timeout=20s;
    server 192.168.1.100:8004 max_fails=3 fail_timeout=20s;
}
```

> - ip_hash：指定请求调度算法，默认是weight权重轮询调度，可以指定
> - server host:port：分发服务器的列表配置
> - -- down：表示该主机暂停服务
> - -- max_fails：表示失败最大次数，超过失败最大次数暂停服务
> - -- fail_timeout：表示如果请求受理失败，暂停指定的时间之后重新发起请求

##### 附2 Linux下nginx安装

1. 包下载

   http://nginx.org/en/download.html

   解压：tar -zxvf nginx-1.18.1.tar.gz

2. nginx通过源码包发布，nginx是c写的，因此需要安装c/c++编译器，**没有安装需要按安装**

   ```shell
   yum install gcc
   yum install gcc-c++
   ```

3. 安装nginx 依赖pcre library

   包下载

   https://ftp.pcre.org/pub/pcre/

   解压：tar -zxvf pcre-8.0.0.tar.gz

   安装：

   ```shell
   #编译
   [root@localhost pcre-8.00]# ./configure
   #安装
   [root@localhost pcre-8.00]# make install
   ```

4. 安装nginx 依赖zlib

   包下载

   http://www.zlib.net/

   解压：tar -zxvf zlib-1.2.11.tar.gz

   安装：

   ```shell
   #编译
   [root@localhost zlib-1.2.11]# ./configure
   #安装
   [root@localhost zlib-1.2.11]# make install
   ```

5. 安装nginx

   ```shell
   #编译
   [root@localhost nginx-1.8.1]# ./configure
   #安装
   [root@localhost nginx-1.8.1]# make install
   ```

6. 启动

   ```shell
   [root@localhost nginx-1.8.1]# /usr/local/nginx/sbin/nginx
   ```

   报错：

   ```she
   ./nginx: error while loading shared libraries: libpcre.so.1: cannot open shared object file: No such
   ```

   解决：

   ```she
   [root@localhost nginx-1.8.1]# find / -type f -name *libpcre.so.* #查看libpcre.so文件位置
   /var/lib/docker/overlay2/388ff1ab7135c525481b252afa17044a5ce188a22692b40a6b7a988d1242d8d2/diff/lib/x86_64-linux-gnu/libpcre.so.3.13.3
   /var/lib/docker/overlay2/08dffdcb93af15356d676da9c2b14ec7566e98765f5a20a1d885595c4a1cf77f/diff/lib/x86_64-linux-gnu/libpcre.so.3.13.3
   /var/lib/docker/overlay2/69841fd2746c2796b68766802fe0b68bde76fde1dc5575515c5ee645ad2c92f7/diff/lib/x86_64-linux-gnu/libpcre.so.3.13.3
   /usr/lib/libpcre.so.1.2.0
   /usr/lib64/libpcre.so.1.2.0
   /usr/local/lib/libpcre.so.0.0.1
   /opt/pcre-8.00/.libs/libpcre.so.0.0.1
   [root@localhost nginx-1.8.1]# ln -s /lib64/libpre.so.0.0.1 /opt/pcre-8.00/.libs/libpcre.so.0.0.1  #建立软链接
   ```

   