# 链接

```
感谢以下几位博主的博客，帮助我自己搭建了termux上的WordPress博客，如果看本篇引文有不理解的，建议详细了解下面链接的文章内容
```

https://www.cnblogs.com/huanghongzhe/p/16240641.html

https://blog.csdn.net/qq_17802895/article/details/113788739

https://www.jianshu.com/p/ae77ebe53aeb

https://blog.csdn.net/GreenhandZdl/article/details/108913624

https://blog.csdn.net/qq_21808961/article/details/102859280

# ssh连接termux

## 配置termux的ssh

### 请读写权限

```
termux-setup-storage
```



### 安装openssh

```
apt update
apt upgrade

apt-get update
apt install openssh
```



### 启动ssh

```
sshd
```

至此手机端已经搞定



## 配置登录密钥

### 电脑新建密钥

首先电脑要装ssh工具，然后进入cmd

```
ssh-keygen
```


生成密钥后，进入电脑administartor目录，拷贝.pub文件至手机的根目录，也就是/storage/emulated/0

### 拷贝移动密钥

```
cd ~/.ssh
cp /sdcard/[文件名].pub ./
cat [文件名].pub >> authorized_keys
```


至此全部任务已经完成

最后让我们获取termux的用户名和密码来通过ssh工具连接

## 获取账号信息

### 获取用户名

```
whoami
```



### 修改密码

```
passwd
```



### 查看ip地址

```
ifconfig -a
```

ifcofig运行效果如下:

```
dummy0: flags=195<UP,BROADCAST,RUNNING,NOARP>  mtu 1500
        inet6 fe80::bc05:1ff:fe55:4556  prefixlen 64  scopeid 0x20<link>
        unspec 00-00-00-00-00-00-00-00-00-00-00-00-00-00-00-00  txqueuelen 1000  (UNSPEC)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 66  bytes 4620 (4.5 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
......这里省略部分信息.
wlan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.43.1  netmask 255.255.255.0  broadcast 192.168.137.255
        inet6 fe80::76d2:1dff:fe00:73fd  prefixlen 64  scopeid 0x20<link>
        unspec 00-00-00-00-00-00-00-00-00-00-00-00-00-00-00-00  txqueuelen 1000  (UNSPEC)
        RX packets 1824  bytes 1096921 (1.0 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 2529  bytes 366296 (357.7 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

这里的`wlan0`中的** inet 192.168.43.1**中的`192.168.43.1`就是**当前手机的ip地址**

```
wlan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.43.1  netmask 255.255.255.0  broadcast 192.168.137.255
```



## ssh连接

语法格式如下

```
ssh [username]@[host] -p [port] #termux ssh默认端口为8022
```

原文链接：https://blog.csdn.net/qq_17802895/article/details/113788739



# Xshell连接termux



Xshell创建秘钥

打开Xshell，点击`工具`，`新建用户密钥生成向导`

![在这里插入图片描述](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202209280021422.png)

然后选择`秘钥类型`和`秘钥长度`,默认即可,点击下一步

![在这里插入图片描述](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202209280021997.png)

等待秘钥生成结束后,继续点击`下一步`

![img](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202209280022463.png)

输入`秘钥名称`和`秘钥密码`,继续点击`下一步`

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191101160534926.png)

### Xshell保存公钥

此时可以看到公钥了,点击`存为文件`

![在这里插入图片描述](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202209280023414.png)

保存到电脑上的用户目录下的.[ssh](https://so.csdn.net/so/search?q=ssh&spm=1001.2101.3001.7020)目录下:

![img](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202209280023182.png)

### 导出私钥并保存

然后就看看到创建好的用户秘钥了

![在这里插入图片描述](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202209280023392.png)

导出私钥

![在这里插入图片描述](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202209280024422.png)

输入密码

![在这里插入图片描述](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202209280024819.png)

## 将秘钥设置到Termux

### Termux访问4@`手机存储器`


会在用户主目录下生成storage目录,storage目录下的shared目录对应我们手机内部存储的根目录(/storage/emulated/0/),我们通过文件资源浏览器打开的就是这个/storage/emulated/0/目录,只不过在Termux中/storage/emulated/0/对应的是storage目录下的shared目录。

### 将公钥发送到手机

我这里将公钥通过`QQ`发送到手机上.



**剩余内容与ssh连接对应模块相同**

原文链接：https://blog.csdn.net/qq_21808961/article/details/102859280

# 配置wordpress



## 一、电脑连接termux

### ssh连接

### Xshelll连接

> 由于在手机上不好操作，所以我们首先需要在termux上安装openssh工具，此步骤也可以忽略。



## 二、更换国内源

复制运行即可:

```shell
sed -i 's@^\(deb.*stable main\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/termux-packages-24 stable main@' $PREFIX/etc/apt/sources.list
sed -i 's@^\(deb.*games stable\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/game-packages-24 games stable@' $PREFIX/etc/apt/sources.list.d/game.list
sed -i 's@^\(deb.*science stable\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/science-packages-24 science stable@' $PREFIX/etc/apt/sources.list.d/science.list
apt update && apt upgrade
```

另外还装一个unzip,后面要用

```powershell
pkg install unzip
```

## 三、安装mysql

> 由于mysql被甲骨文公司收购后有闭源的风险，所以社区开发了MariaDB

```powershell
pkg install mariadb
```

**初始化数据库**

```powershell
mysql_install_db
```

注意早期的termux需要初始化数据库，现在已经自动初始化了。

**启动数据库**

```powershell
nohup mysqld &
```

运行后nohup 会提示错误，这是正常的，不用管：

**nohup: ignoring input and appending output to `nohup.out'**

### 停止mysql

> 因为mysql启动后会像debug一样一直开着，所以我们用杀死进程的方法来停止mysql

#### 方法一

1.首先获取进程PID号

```powershell
ps aux | grep mysql
```

![参数用法](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202209280027246.png)
2.然后杀死进程

```powershell
kill -9 [PID]
```

#### 方法二

搜索pid号比较麻烦，除了上面的方法还可以这样终止进程

```powershell
kill -9 `pgrep mysql`
pkill mysql
```

### 登录mysql

> 注意，登录前先启动mysql服务
> 两个用户，一个termux用户名的用户(密码为空)，一个root用户.

#### 登录普通用户

```powershell
mysql -u $(whoami)
```

登录 termux用户 数据库 mysql -u $(whoami)
如果出现ERROR 2002 (HY000): Can’t connect to local MySQL server through socket ‘/data/data/com.termux/files/usr/tmp/mysqld.sock’ (111)类似错误，请执行mysqld_safe，然后通过侧边栏的new session再开一个终端，重试mysql -u $(whoami) 如果还是不行，你还是谷歌吧。
原文链接：https://blog.csdn.net/GreenhandZdl/article/details/108913624



修改另一个账户root的密码

```powershell
# 登录 Termux 用户
mysql -u $(whoami)

# 修改 root 密码的 SQL语句
use mysql;
set password for 'root'@'localhost' = password('你设置的密码');

# 刷新权限 并退出
flush privileges;
quit; 
```

#### 登录root用户

```powershell
mysql -u root -p
```









作者：xnllc
链接：https://www.jianshu.com/p/ae77ebe53aeb
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。





## 四、安装nginx

```powershell
pkg install nginx
```

检查配置文件是否正常

```powershell
nginx -t
```

> 因为刚安装，现在检查肯定是没问题，等我们修改完配置文件，再回过头来检查一遍。

启动nginx

```powershell
nginx
```

**Termux 在 Nginx 上默认运行的端口号是 8080**

可以使用pgrep查看nginx的进程pid号:

```powershell
pgrep nginx
```

如果是本机就直接打开浏览器访问：

```powershell
http://127.0.0.1:8080
```

如果是ssh连接的就访问

```powershell
http://[ip地址]:8080
```

可以使用`ifconfig -a`查看ip地址

**重启nginx**

```powershell
nginx -s reload
```

**停止nginx**

1.以nginx提供的原生方法：

```powershell
nginx -s stop #直接停止

nginx -s quit #完成已经接受的请求，然后退出。
```

2.杀死进程：

```powershell
kill -9 `pgrep nginx`
```

or

```powershell
# 查询 nginx 进程相关的 PID 号
pgrep nginx

# 杀掉 查询出的 PID号进程
kill -9 PID
```

**配置nginx**l

```powershell
vim $PREFIX/etc/nginx/nginx.conf
```

1.添加 inde/data/data/com.termux/files/usr/share/nginx/htmx.php 到默认首页的规则里面
![在这里插入图片描述](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202209280027649.png)
2.取消 `location ~ \.php$` 这些注释，改成图片上面的样子：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210217185636324.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzE3ODAyODk1,size_16,color_FFFFFF,t_70)
Termux 里面的 Nginx 默认网站的根目为：`/data/data/com.termux/files/usr/share/nginx/html`

如果想要修改默认路径的话 只需要在上图配置文件中 替换2处出现的这个路径即可。

## 五、安装php-fpm

> 由于nginx只是一个web服务器，不能够处理php请求，所以要安装php-fpm

测试php解析前先安装php

```powershell
pkg install php
pkg install php-fpm
```

编辑 php-fpm 的配置文件`www.conf`

```powershell
vim $PREFIX/etc/php-fpm.d/www.conf
```

定位搜索 `listen =` 找到

```powershell
listen = /data/data/com.termux/files/usr/var/run/php-fpm.sock
```

改为：

```powershell
listen = 127.0.0.1:9000
```

**测试php解析**

> 需要先完成nginx和php-fpm的安装和配置

在这个网站根目录下：

```powershell
/data/data/com.termux/files/usr/share/nginx/html
```

新建 info.php 内容为：<?php phpinfo(); ?>

先启动`php-fpm`,然后启动`nginx`,如果你的 Nginx 已经启动了的话，使用 `nginx -s reload` 重启 Nginx.

本机访问:[点这里](http://127.0.0.1:8080/info.php)
如果是ssh连接的则:

```powershell
http://[ip地址]:8080/info.php
```

好的咋们上面完成了:

1. mysql的安装和配置
2. nginx的安装和配置
3. php的安装
4. php-fpm的安装和配置
5. 测试了php解析
6. 测试了nginx服务器的正常运行

**正式开始搭建wordpress**

## 六、新建数据库



```bash
~ $ whoami
u0_a205
~ $ mysql -u u0_a205 -p
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 5
Server version: 10.6.4-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]>
```



3 . 创建wordpress数据：create database wordpress;show databases;



```bash
MariaDB [(none)]> create database wordpress;show databases;
Query OK, 1 row affected (0.001 sec)

+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| test               |
| wordpress          |
+--------------------+
6 rows in set (0.002 sec)
```

1. 创建一个root1 密码为root的用户。（用户名和密码都随意主要是用户的权限，默认的两个用户只能localhost链接，不够用。）

```sql
GRANT ALL PRIVILEGES ON *.* TO 'root1'@'%' IDENTIFIED BY 'root' WITH GRANT OPTION;
```

```bash
MariaDB [(none)]> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A
MariaDB [mysql]> SELECT user,host,Password  FROM mysql.user;
+-------------+-----------+----------+
| User        | Host      | Password |
+-------------+-----------+----------+
| mariadb.sys | localhost |          |
| root        | localhost | invalid  |
| u0_a205     | localhost | invalid  |
|             | localhost |          |
+-------------+-----------+----------+
4 rows in set (0.003 sec)
MariaDB [mysql]> GRANT ALL PRIVILEGES ON *.* TO 'root1'@'%' IDENTIFIED BY 'root' WITH GRANT OPTION;
Query OK, 0 rows affected (0.004 sec)

MariaDB [mysql]> SELECT user,host,Password  FROM mysql.user;
+-------------+-----------+-------------------------------------------+
| User        | Host      | Password                                  |
+-------------+-----------+-------------------------------------------+
| mariadb.sys | localhost |                                           |
| root        | localhost | invalid                                   |
| u0_a205     | localhost | invalid                                   |
|             | localhost |                                           |
| root1       | %         | *81F5E21E35407D884A6CD4A731AEBFB6AF209E1B |
+-------------+-----------+-------------------------------------------+
5 rows in set (0.003 sec)

MariaDB [mysql]>
```

到这里 数据库部分准备完成，



## 七、下载 WordPress



## 第二部分：wordpress 安装

1. 下载wordpress5.9版本代码，并且解压到nginx相关目录。

```powershell
# 移动到 nginx 网站根目录下
cd $PREFIX/share/nginx/html

#  wget 下载
wget https://cn.wordpress.org/wordpress-6.0.2.zip

# unzip 解压 
mv wordpress/ $PREFIX/share/nginx/html

unzip wordpress-6.0.2.zip  -d $PREFIX/share/nginx/html && cd $PREFIX/share/nginx/html/wordpress  && chmod -Rf 777 ./*
```



/data/data/com.termux/files/usr/share/nginx/html

检查启动:

1. mysql
2. php-fpm
3. nginx

> 建议先退出termux，然后重新启动

```powershell
nohup mysqld &

php-fpm

nginx
如果开启了，就nginx -s reload

不要重复执行命令！！！
```

## 八、安装wordpress

浏览器访问: `http://127.0.0.1/wordpress/`进行 WordPress 的安装

如果不是使用本机访问的，而是和我一样使用ssh连接的则需要把上述链接种的ip地址换成手机局网ip地址，`ifconfig -a`查看ip

![](https://upload-images.jianshu.io/upload_images/2861088-3c9236bcbca208c4.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)



使用之前新建了的数据库，和用户名密码，主机这里用localhost不好使，换成对应ip

![img](https://upload-images.jianshu.io/upload_images/2861088-aaa82093c9bc4b1a.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

![img](https://upload-images.jianshu.io/upload_images/2861088-781f9f3b8265f43f.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)



![image-20220928030428207](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202209280304297.png)

# termux内网穿透





## 提要

如果您希望在Android中运行termux终端并执行一系列小型Linux服务，就会考虑到下一个问题：我如何远程管理它？这时候，就是cpolar内网穿透工具出场的时候了。它让您可以在任何地点，管理你的termux环境容器。

## 1. 手机中安装termux终端APP（不需要root）

Termux是一款强大的安卓终端模拟APP，无需root直接启动，自动安装最小化linux系统，支持apt管理软件包，完美支持python,ruby,go,nodejs。

## 2. 在termux中安装dnsutils工具包(必要）

```shell
  apt install dnsutils
```

Shell

Copy

它会创建一个DNS解析文件，路径在$PREFIX/etc/resolv.conf，里面有配置DNS解析服务器地址（默认已经加了8.8.8.8）

## 3. 下载最新的cpolar客户端(ARM 版本）

```shell
curl -O -L https://cpolar.com/static/downloads/cpolar-stable-linux-arm.zip
```

Shell

Copy

## 4. 解压缩

```shell
unzip cpolar-stable-linux-arm.zip
```

Shell

Copy

## 5. 认证token，在cpolar后台复制你自己的token值

### 5.1 访问官网 https://www.cpolar.com，注册cpolar帐号

![image_1dbjdli0112q12tb38q1oso19an9.png-231.6kB](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202210082058708.png)

### 5.2 登录后台，获取token值

登录cpolar后台仪表盘：https://dashboard.cpolar.com/，验证菜单里，复制你的token值
![image_1dbjdr93d1vuglva27d1f05s88m.png-136.5kB](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202210082058560.png)

### 5.3 认证token，在termux终端输入如下命令：

粘贴自己的token

```
./cpolar authtoken xxxxxxxxxxtokenxxxxxxx
```

## 6. 内网穿透举例

### 6.1 映射8080端口到外网

```shell
./cpolar http 8080
```

Shell

Copy

![image_1dbjdu6mp15sj1nlqd1k6tb7p13.png-53.7kB](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202210082058596.png)

### 6.2 外网远程ssh控制

```shell
./cpolar tcp 22
```

Shell

Copy

### 6.3 更多的关于cpolar的命令的参数及功能介绍

请参阅cpolar官网的在线文档及使用教程案例。

在线文档：https://cpolar.com/docs
教程案例：https://cpolar.com/blog

## 7. 总结：

我们介绍了如何在termux中安装cpolar，您可以使用它内网穿透ssh管理android主机，或者映射一个web站点到公网。
稍后我们将介绍更多关于termux的有趣内容





# 修改WordPress地址（URL）地址后导致网页无法访问，后台进不去



网上有多种解决方法，下面我介绍我成功解决得一种方法。
可以通过任意工具（在阿里云服务器远程登录或者从宝塔进入什么得都行）找到自己Linux服务器得文件夹
然后找到建站主题得文件夹
www->wwwroot->然后域名文件夹->wp-content->themes->下面会有几组主题文件夹，逐一打开，找到functions.php，然后双击查看里面的内容，应该会有一个与其他几个明显不同，这个就是当前应用的主题了。
————————————————
版权声明：本文为CSDN博主「撑一把纸伞.」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_50882544/article/details/121501006

![image-20220929003032631](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202210082100718.png)

![image-20220929003051599](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202210082100952.png)

然后在空白处加入下面的代码，然后保存

```
update_option('siteurl','http://自己的域名');   
update_option('home','http://自己的域名');
```

