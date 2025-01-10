[博主的个人网站](http://www.sdshui.club/)

[Node.js下载地址](https://nodejs.org/)

## 1、下载Node.js

### 设置下载地址

![image-20220817133735500](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202302042351279.png)

### 使用默认环境变量

![image-20220817133800065](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202302042351417.png)

**等待安装完成即可**




##2、准备工作

### 打开node.js的下载目录

![image-20220817133830019](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202302042351408.png)



### 新建两个文件夹 node_global和node_cache

**目的：配置npm在安装全局模块时的路径和缓存cache的路径**

![image-20220817133851641](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202302042351303.png)

### 在cmd命令下执行如下两个命令：

```
npm config set prefix "C:\Node\node_global"

npm config set cache "C:\Node\node_cache"
```



## 3、安装vue包

### 以管理员身份运行命令提示符

**输入**

```
npm install -g vue
```

**如图**


![image-20220817134514150](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202302042351321.png)

### 检查安装是否成功

**在node_global文件夹里出现node_modules文件，表明安装成功**

![image-20220817133949867](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202302042351647.png)



## 4、环境变量的配置



### 1配置用户变量

**默认情况下，在用户变量中会出现Node.js的默认PATH配置如下**

![image-20220817134844309](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202302042351777.png)

**将该变量内容，修改为 node_global的地址**

![image-20220817134936423](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202302042351922.png)

#### 如下

![image-20220817134803346](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202302042351951.png)



### 2新建系统变量NODE_PATH



#### 首先打开node_global

![image-20220817134410893](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202302042351961.png)

**复制node_mdules的地址**



#### 在系统变量新建NODE_PATH变量名

**并将node_mdules的地址，填入变量名**

![image-20220817134337200](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202302042351005.png)

### 3配置系统变量名Path

打开Path后发现 **最后一行** 有 Node.js默认配置的 C:\Node\

![image-20220817134302036](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202302042351174.png)

**在 变量C:\\Node\\ 行下面添加 添加%NODE_PATH%**



```
%NODE_PATH%
```



![image-20220817134219021](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202302042351293.png)

**以上完成环境变量配置**





## 5、测试

### 打开命令提示符

**输入**

```
node
```

**如图**

![image-20220817134159500](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202302042351493.png)

**再输入**

```
require('vue')
```

**出现如下界面则，则配置成功**

![image-20220817134135395](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202302042351470.png)
