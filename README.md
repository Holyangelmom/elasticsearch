# 单节点部署


1、前言  


```
  由于最初安装的es1.4没有兼容es-sql，es-sql提供了sql的方式对es中的索引进行检索，所以换到了es2.3.1版本，谁知跳进了广西大石围天坑之中，费了九牛二虎之力才从坑里爬出来。好了不扯了，言归正传。
```

2、Prerequest  


```
  （1）centos6.8 minimal

  （2）已创建普通用户，我的用户是panaidan

  （3）maven
```

3、下载  


```
  从官网或者github下载2.3.1版本
```

4、安装  


```
  解压

  移动

  配置文件${ES\_HOME}/config/elasticsearch.yml的配置：

  network.host: \[\_local\_, 127.0.0.1, 192.168.127.x\]   //配置可通过localhost、本地回环、ip访问ES

  http.port: 9200

  环境的配置：

  （1）修改内存锁定限制 /etc/security/limits.conf

            panaidan    soft    memlock    unlimited

            panaidan    hard    memlock    unlimited

  （2）修改最大文件打开数

             panaidan    soft    nofile    65536

            panaidan    hard    nofile    65536

  （3）允许其他主机访问9200端口

   第一种做法是直接关闭防火墙，但是不建议：

   sudo service iptables stop

   sudo chkconfig iptables off

   第二种做法是添加规则，允许主机访问9200端口，也可以限制哪些主机可以访问等等，不懂赶紧脑补防火墙知识：

   sudo vim /etc/sysconfig/iptables

   插入规则：-A INPUT -m state --state NEW -m tcp -p tcp --dport 9200 -j ACCEPT
```

5、启动

```
 sh ${ES\_HOME}/bin/elasticsearch   或者  sh ${ES\_HOME}/bin/elasticsearch -d （后台启动）
```

6、插件安装

```
 （1）head

          联网安装：sh ${ES\_HOME}/bin/plugin install mobz/elasticsearch-head

          本地安装：sh ${ES\_HOME}/bin/plugin install file:/home/panaidan/file/elasticsearch-head.zip

           访问：ip:9200/\_plugin/head

 （2）sense

          因为sense集成在kibana当中，所以需安装kibana。下载kibana，解压、移动。安装sense：sh ${kibana\_home}/bin/kibana plugin --install elastic/sense。

          启动kibana，因为kibana不能以后台形式启动，它的日志一直在终端输出，所以这里使用"&"命令将kibana以后台形式运行，并将错误输出重定向标准输出中 :

          sh ${kibana\_home}/bin/kibana -Q -e http://ip:9200 & 2&gt;&1

  （3）ik

           到github下载相应源码，解压，进入ik根目录，执行:mvn clean package。

           打包好了之后创建ik目录：mkdir   ${ES\_HOME}/plugins/ik.

           拷贝：cp ${ik\_home}target/releases/elasticsearch-analysis-ik-{version}.zip ${ES\_HOME}/plugins/ik。

           解压：unzip elasticsearch-analysis-ik-{version}.zip。
```



