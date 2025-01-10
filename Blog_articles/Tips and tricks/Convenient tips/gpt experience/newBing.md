浏览器：edge dev

插件： header-editor

方式：插件中新添加配置文件

规则类型：修改请求头 

匹配类型：正则表达式 

匹配规则： ^http(s?)://(.*).bing\.com/(.*) 

头名称：x-forwarded-for 

头内容：1.1.1.1