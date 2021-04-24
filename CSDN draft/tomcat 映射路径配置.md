[TOC]

[原文: https://www.cnblogs.com/pyrmkj/p/4434200.html](https://www.cnblogs.com/pyrmkj/p/4434200.html)

#### 1. 默认配置

位置：apache-tomcat7-8088\conf\server.xml

```xml
<Host name="localhost"  appBase="webapps"
      unpackWARs="true" autoDeploy="true">
```

> appBase:可以指定绝对目录,也可以指定相对于<CATALINA_HOME>的相对目录.如果没有此项,默认为<CATALINA_HOME>/webapps 

以上定义了默认根目录为"webapps"（相对路径，相对于<CATALINA_HOME>）

#### 2. 指定映射路径

位置：apache-tomcat7-8088\conf\server.xml

```xml
<Host name="localhost"  appBase="webapps"unpackWARs="true" autoDeploy="true">
    <Context path="" docBase="ROOT" debug="0"/>
    <!--<Context path="/sample" docBase="sample" debug="0" reloadbale="true"/>-->
    <Context path="/hz7" docBase="C:\work\hz7\target\hz7-0.0.1-SNAPSHOT" debug="0" reloadbale="true"/>
```

可以通过在Host标签下加上多个`Context tag`来创建自定义访问。一般我们用Context来覆盖Host的appBase的设定而不是直接修改appBase。

> path: Host的虚拟目录
>
> docBase: 映射的物理目录的地址，可指定相对路径，相对appBase下，也可以指定绝对路径（例如：C:\work\hz7\target\hz7-0.0.1-SNAPSHOT）。如果无此项则默认为appBase/ROOT 。

> Context 一：
>
> path=""（注意不是path="/")，则相当于是这个Host的虚拟根目录，docBase则指出了物理目录的位置为相对于appBase下的ROOT目录。因此在访问在浏览器中输入http://localhost:8080/，则访问到的是webapps下的ROOT目录。
>
> Context 二：
>
> 如果你访问http://localhost:8080/sample/ ,则会访问到appBase/sample里的内容。
>
> Context 三：
>
> path=""，指的是这是Host的虚拟根目录，而docBase指出物理目录是个绝对地址，因为如果你访问[http://localhost:8080/](http://localhost:8080/sample/) ，则会访问到的是C:\work\hz7\target\hz7-0.0.1-SNAPSHOT 里的内容 。