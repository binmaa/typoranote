**同一局域网内终端使用ip_hash负载均衡策略，访问全部分发到同一个后端服务器**

现象：

> Nginx和客户端处于同一个局域网，使用ip_hash负载均衡策略，不同客户端的请求全部都分发到同一个后台服务器

原因：

> 将客户端ip转化成C类网络地址，然后将该网络地址当作hash关键字，来保证这个客户端请求总是被转发到一台服务器上
>
> C类网络：*C类*IP*地址*是指，在IP*地址*的4段号码中，前3段号码为*网络*号码，剩下的1段号码为本地计算机的号码
>
> **Nginx的ip_hash算法都将一个ip地址的前三段作为hash的关键字。**

解决：

修改 nginx源码文件重新编译运行。`nginx-1.8.1.tar.gz/src/http/modules/ngx_http_upstream_ip_hash_module.c`

```c
80 static u_char ngx_http_upstream_ip_hash_pseudo_addr[3];
//3改成4 static u_char ngx_http_upstream_ip_hash_pseudo_addr[3];

121 case AF_INET:
122    sin = (struct sockaddr_in *) r->connection->sockaddr;
123    iphp->addr = (u_char *) &sin->sin_addr.s_addr;
124    iphp->addrlen = 3;//3改成4 iphp->addrlen = 4;
125    break;

135    default:
136        iphp->addr = ngx_http_upstream_ip_hash_pseudo_addr;
137        iphp->addrlen = 3;//3改成4 iphp->addrlen = 4;
138    }
```

按照上述编译运行可能出现的问题：
```nginx
./nginx: error while loading shared libraries: libpcre.so.1: cannot open shared object file: No such
```
[https://blog.csdn.net/weixin_44297303/article/details/89505398?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.control&dist_request_id=&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.control](https://blog.csdn.net/weixin_44297303/article/details/89505398?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.control&dist_request_id=&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.control)