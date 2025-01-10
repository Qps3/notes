# Ubuntu运行JavaMaven项目



## 一、编译运行项目

### 1. Ubuntu搭建Java环境

Ubuntu是一个流行的Linux服务器操作系统。在Ubuntu上搭建Java开发环境主要包括安装JDK和配置环境变量。

```bash
# 更新Ubuntu软件源索引
sudo apt update 

# 安装默认的OpenJDK 11
sudo apt install openjdk-11-jdk
```

安装完成后,可以使用以下命令检查Java是否安装成功:

```bash
java -version
```

下一步需要配置Java环境变量,打开`/etc/profile`文件:

```bash
sudo vim /etc/profile
```

在文件末尾添加以下环境变量:

```bash
# 设置JAVA_HOME
JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64 
export JAVA_HOME

# 将JDK的bin目录添加到PATH
PATH=$PATH:$JAVA_HOME/bin
export PATH
```

保存文件后,运行以下命令使环境变量立即生效:

```bash
source /etc/profile
```

现在Java开发环境搭建完成,可以开始编写和运行Java代码了。

### 2. Ubuntu上编译运行Java Maven

在Ubuntu上编译并运行Java项目,常用的构建工具是Maven。以下是在Ubuntu上使用Maven运行一个示例项目的步骤:

#### 2.1 安装Maven

```bash
# 更新软件源索引
sudo apt update

# 安装Maven
sudo apt install maven
```

检查Maven是否安装成功:

```bash
mvn -version
```

#### 2.2 创建Maven项目

==注意：此处作者是自己在win上写的mavn项目==

> - 直接上传到ubuntu服务器所以没有在ubuntu生成项目骨架
>
> - 而是直接对项目进行了编译运行

使用Maven archetype生成一个项目骨架:

```bash 
mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app
```

这将创建一个目录`my-app`,进入该目录就是一个可以编译运行的Maven项目。

#### 2.3 编译项目

```bash
cd my-app
mvn compile
```

#### 2.4 运行项目

```bash
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

以上命令将会运行`com.mycompany.app.App`这个主类来启动项目。

至此,一个基本的Maven项目在Ubuntu上就可以编译运行了。后续可以根据需要修改代码和配置,开发自己的项目。



## 二、遇到的问题与解决





### 3. 无法在本地访问云服务器8080端口

在云服务器(例如EC2)上运行一个应用,监听8080端口。但是在本地浏览器无法访问该应用。问题可能在于:

#### 3.1 ping远程服务器网络

使用`ping`命令检查本地机器是否可以连接到服务器:

```bash
ping server_ip
```

如果ping不通,需要检查本地和云服务器是否是否有防火墙等设置在阻止通信。



#### 3.2 检查服务器端口是否开放

在服务器上检查是否有防火墙阻止了8080端口的访问:

```bash
# Ubuntu使用ufw作为防火墙
sudo ufw status

# 如果端口未开放,使用以下命令开放
sudo ufw allow 8080/tcp
```



#### 3.3 查看自己的应用是否仅监听localhost

1. 有些应用默认只监听localhost
   - 需要更改配置,使应用监听0.0.0.0或公网IP。

2. 以Spring Boot应用为例:

   - ```ynl
     server:
       address: 0.0.0.0
     ```



#### 3.4 查看云服务器实例的安全组设置

如果使用的是AWS、Azure、GCP等IaaS提供商,需要在其控制台的**安全组(Security Group)**中开放8080端口。

```js
// 添加一条入站规则,允许来自任何IP地址的TCP连接访问8080端口
inbound rule: 
  protocol: TCP 
  port: 8080
  source: 0.0.0.0/0
```



### 4. 修改HTML无法生效的问题

在开发过程中,如果修改了后端服务生成的HTML,但通过浏览器访问时发现没有修改成功,常见原因包括:

#### 4.1 清除浏览器缓存

可以尝试清空浏览器缓存后刷新页面,或者使用Ctrl/Command + F5进行强制刷新。

#### 4.2清除服务端缓存

某些服务器或应用框架会缓存HTML页面。需要清除服务端缓存才能让修改立即生效。

```java
// Spring Boot应用,可以添加此配置禁用模板缓存
spring.thymeleaf.cache=false 
```

#### 4.3 静态资源版本

有些应用会在静态资源后添加版本号或Hash值,需要递增版本号才能加载新的资源。

```html
<script src="/js/app.js?v=2"></script>
```

#### 4.4 应用没有重新部署

==这里是作者遇到的问题，并通过重新部署得到解决==

对HTML的修改需要重新构建并部署应用才能生效。

1. 重新编译项目

```bash
cd my-app
mvn compile
```

2. 重新运行项目

```bash
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```



#### 4.5 清除CDN缓存

如果使用CDN,可能需要清除CDN缓存才能获取修改后的文件。





## 三、补充内容：





### 1. Ubuntu搭建Java环境 

Ubuntu上搭建Java开发环境,可以选择使用Oracle JDK或开源的OpenJDK。本文使用OpenJDK的主要原因是:

- OpenJDK完全开源,安装和升级更方便。Oracle JDK需要登录Oracle账号并接受许可协议。

- OpenJDK与Oracle JDK具有相同的性能表现,但OpenJDK的许可更为灵活。

- Ubuntu软件仓库中内置OpenJDK,可以通过简单的apt命令安装。

### 3. 无法在本地访问云服务器上8080端口

不同的云服务提供商,都可以在其安全组(Security Group)或网络设置中开放端口:

- **AWS**: 在安全组添加入站规则,允许访问端口

- **Azure**: 在网络安全组(NSG)添加入站安全规则

- **GCP**: 为防火墙添加入站规则来开放端口

- **阿里云**: 在安全组中增加入方向访问策略

- **腾讯云**: 在安全组添加入站和出站规则

