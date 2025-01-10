声明周期的意思是变量实际的存在时间，也就是变量在被删除之前在内存中停留多久
作用域就是我们可以访问这个变量的范围

*static local variable*允许我们声明一个变量，它的周期是整个程序的生存期，但是作用域被限制在这个函数里。

![](./storage%20bag/Pasted%20image%2020230703094220.png)

static 创造的变量是放在*heap*里的，默认值 0