[git建立本地仓库并上传代码到GitHub](https://www.cnblogs.com/vofill/p/13036003.html)

# 一、新建项目并提交代码

　　1、在本地需要上传的文件夹右击选择git bash here

　　2、输入个人信息　

　　　　git config --global user.name "xxxx" 

　　　　git config --global user.email "xxxxx@qq.com"

　　3、在本地项目目录创建本地仓库

　　　　git init

　　　　输入完命令后项目目录会有一个隐藏的.git文件夹

　　4、上传所有代码到本地仓库

　　　　git add .(后面这个点不能少了)

　　5、代码上传到本地仓库后，执行提交命令

　　　　git commit - m "项目名称"

　　6、在GitHub上新建一个repository

　　7、关联本地仓库并上传代码

　　　　git remote add origin https://github.com/vofill/test.git(新建repository后会有一个地址)

　　8、最后执行上传推送命令

　　　　git push origin master

　　完成代码上传GitHub

# 二、更新修改代码

　　1、先进入到项目根目录下：cd D:\test 查看是否有冲突：
　　　　git status
　　2、将当前工作目录中更改或者新增的文件加入到Git的索引中，加入到Git的索引中就表示记入了版本历史中
　　　　git add .
　　3、添加注释（必填）：
　　　　git commit -m “注释内容”
　　4、提交本次修改到远程仓库

　　　　git push https://github.com/vofill/test.git

三、从GitHub上通过命令down代码

　　1、打开git，进入要存放项目代码的git目录

　　2、使用命令

　　　　git clone git://github.com/vofill/test.git

　　　　或者git clone https://github.com/vofill/test.git

四、从GitHub上通过命令update代码

　　1、打开git，进入要存放项目代码的git目录

　　2、使用命令git pull