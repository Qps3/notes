HBase总结如下

1. 下载解压hbase到mudule文件夹
2. 为hbase添加环境变量，使用hbase version验证，并分发到集群
3. source /etc/ profile.d/my_env.sh生效
4. 修改hbase/conf下的文件
   - ![image-20230124014830172](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/博客图片总类/java/202301240148531.png)
   - ![image-20230124014959256](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/博客图片总类/java/202301240149367.png)
   - ![image-20230124015024238](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/博客图片总类/java/202301240150319.png)
     - ![image-20230124015052090](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/博客图片总类/java/202301240150144.png)
5. 修改hbase / conf / regionservers文件
6. 上述执行完成之后，注意集群的分发

