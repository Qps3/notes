# 理解根目录，getClass().getResourceAsStream和getClass().getClassLoader().getResourceAsStream的区别

[后端](https://codeantenna.com/tag/后端)

------

web项目中 ，编译好的.class文件，xml、properties等配置文件都在 **WEB-INF/classes**目录下。

也就是编译后的根目录。

**getClass().getClassLoader().getResourceAsStream** ，通过类加载器可以直接定位到根目录下。（因为双亲委派机制）

**getClass().getResourceAsStream** 则是定位到当前类的平级。（也就是当前类所在包）

eg：

![img](assets/202209091649470.png)

分别访问两个配置文件：

```java
this.getClass().getClassLoader().getResourceAsStream("abc.properties")
```

 

```java
new Test().getClass().getResourceAsStream("biabc.properties")
```







# java之DocumentBuilderFactory解析xml

码莎拉蒂 .

于 2018-06-01 22:36:13 发布

13366
 收藏 8
分类专栏： Java 文章标签： java DocumentBuilderFactory xml
版权

Java
专栏收录该内容
83 篇文章5 订阅
订阅专栏
1、About documentBuilderFactory API description

1）、 javax.xml.parsers 包DocumentBuilderFactory创建DOM模式的解析器对象, DocumentBuilderFactory是抽象工厂类，不能直接实例化，但是有newInstance方法


2）、DocumentBuilderFactory.newInstance() 得到创建 DOM 解析器的工厂

 

 

 

DocumentBuilderFactory documentBuilderFactory = DocumentBuilderFactory.newInstance()




3）、newDocumentBuilder方法得到 DOM 解析器对象

 

 

 

 

DocumentBuilder documentBuilder = documentBuilderFactory.newDocumentBuilder();




4)、DOM解析器解析输入流，这里可以是XML，文档转化为输入流，或者字符串转为ByteArrayInputStream,DOM 解析器对象的 parse() 方法解析 XML 文档，得到代表整个文档的 Document 对象

 

 

InputStream is = new FileInputStream("xxx.xml");
documentBuilder.parse(is);
documentBuilder.parse(new ByteArrayInputStream(str.getBytes())); 


5)、得到 XML 文档的根节点

 

Element rootElement = document.getDocumentElement();


6)、得到节点的子节点

 

 

 

NodeList studentInfo = appElement.getChildNodes();








2、Test Demo








Student.java文件

package com.parse.doc;

public class Student {
	
	public String name;
	public String age;
	public String sex;
	public Student() {}
	
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getAge() {
		return age;
	}
	public void setAge(String age) {
		this.age = age;
	}
	public String getSex() {
		return sex;
	}
	public void setSex(String sex) {
		this.sex = sex;
	}
	@Override
	public String toString() {
		return "name=" + name + ", age=" + age + ", sex=" + sex;
	}
}
ParseXml.java文件

package com.parse.doc;

import java.io.ByteArrayInputStream;
import java.util.ArrayList;
import java.util.List;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;

import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.NodeList;


public class ParseXml {
	
	public static String xml = "<Students><student><name><![CDATA[陈喻]]></name><age><![CDATA[26]]></age><sex><![CDATA[男]]></sex></student><student><name><![CDATA[陈彩凤]]></name><age><![CDATA[25]]></age><sex><![CDATA[女]]></sex></student><student><name><![CDATA[陈紫宣]]></name><age><![CDATA[2]]></age><sex><![CDATA[女]]></sex></student><student><name><![CDATA[陈紫曦]]></name><age><![CDATA[7个月]]></age><sex><![CDATA[女]]></sex></student></Students>";
	public static final String STUDENT = "student";
	public static final String NAME = "name";
	public static final String AGE = "age";
	public static final String SEX = "sex";
	
	public static void main(String args[]) {
		List<Student> students = parseXmlByDoc(xml);
		if (students != null && students.size() > 0) {
			for (Student student : students) {
				System.out.println(student);
			}
		} else {
			System.out.println("students size is 0");
		}
		
	}
	
	public static List<Student> parseXmlByDoc(String str) {
		if (str == null || "".equals(str)) {
			System.out.println("str is null or ''");
			return null;
		}
		DocumentBuilderFactory documentBuilderFactory = DocumentBuilderFactory.newInstance();
		List<Student> students = new ArrayList<Student>();
		DocumentBuilder documentBuilder;
		try {
			documentBuilder = documentBuilderFactory.newDocumentBuilder();
			Document document = documentBuilder.parse(new ByteArrayInputStream(str.getBytes()));
			Element rootElement = document.getDocumentElement();
			NodeList studentNodeList = rootElement.getElementsByTagName(STUDENT);
			for (int i = 0; i < studentNodeList.getLength(); i++) {
				Element appElement = (Element) studentNodeList.item(i);
				NodeList studentInfo = appElement.getChildNodes();
				Student student = new Student();
				for(int j = 0; j < studentInfo.getLength(); j++) { 
					Element element = (Element) studentInfo.item(j);
					String appAttr = element.getTagName();
					switch (appAttr) {
						case NAME:
							student.setName(element.getTextContent());
							break;
						case AGE:
							student.setAge(element.getTextContent());
							break;
						case SEX:
							student.setSex(element.getTextContent());
							break;
						default:
							break;
					} 
				}
				students.add(student);
			}
		} catch (Exception e) {
			e.printStackTrace();
			return null;
		}
		return students;
	}
}








3、Running results


name=陈喻, age=26, sex=男
name=陈彩凤, age=25, sex=女
name=陈紫宣, age=2, sex=女
name=陈紫曦, age=7个月, sex=女



————————————————
版权声明：本文为CSDN博主「码莎拉蒂 .」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/u011068702/article/details/80543576













下面这一坨代码就是将bean中某一个标签以及该标签保存的实例对象保存到Map中去

```
HashMap<String,Object>  beanMap = new HashMap<>(); //保证存储在beanMap中的元素对，id对应的是实例对象


    public dispatcherServlet() {

    }


    public void init(){


        try {

            InputStream inputStream = getClass().getClassLoader().getResourceAsStream("applicationContext.xml");
            //1.创建DocumentBuilderFactory
            DocumentBuilderFactory documentBuilderFactory = DocumentBuilderFactory.newInstance();
            //2.创建DocumentBuilder对象
            DocumentBuilder documentBuilder = documentBuilderFactory.newDocumentBuilder();
            //3.创建Document对象
            Document document = documentBuilder.parse(inputStream);
            //4.获取所有的bean节点
            NodeList beanNodeList = document.getElementsByTagName("bean");


            for (int i = 0; i < beanNodeList.getLength(); i++) {

                Node beanNode = beanNodeList.item(i); //节点类型等于元素节点

                if (beanNode.getNodeType() == Node.ELEMENT_NODE) {
                    Element beanElement = (Element) beanNode;
                    String id = beanElement.getAttribute("id");
                    String className = beanElement.getAttribute("class");
                    Class controllerBeanClass = Class.forName(className);
                    Object beanObj = controllerBeanClass.newInstance();

                    Method setServletContextMethod = controllerBeanClass.getDeclaredMethod("setServletContext", ServletContext.class);
                    setServletContextMethod.invoke(beanObj,this.getServletContext());




                    beanMap.put(id,beanObj);


                }
            }

        } catch (ParserConfigurationException e) {
            throw new RuntimeException(e);
        } catch (IOException e) {
            throw new RuntimeException(e);
        } catch (SAXException e) {
            throw new RuntimeException(e);
        } catch (ClassNotFoundException e) {
            throw new RuntimeException(e);
        } catch (InstantiationException e) {
            throw new RuntimeException(e);
        } catch (IllegalAccessException e) {
            throw new RuntimeException(e);
        } catch (NoSuchMethodException e) {
            throw new RuntimeException(e);
        } catch (InvocationTargetException e) {
            throw new RuntimeException(e);
        }

    }
```



```
1. 根据请求的URL获取SelvertPath， 然后根据SelvertPath解析截取字符串，解析出一个名字

1. 加载配置文件applicatin.xml并读取文件中的bean ，并将bean对应的标签以及标签对应的实例放在map容器中

3. 根据刚才得到的selvertpath中传递的名字去map中找到 对应的处理类Controler,然后调用comtroler类里面的方法（该方法的值由operate的值来决定）


注解被拿掉，就不再是一个servlet组件了

```

```
这样做的好处是，反射代码从controler中提取了出来，并集中放在了dispatcher中去了
```

![image-20220909181902694](assets/202209091819790.png)

卧槽原来如此。。。子类不是Servlet初始化父类的时候init就是一个普通的成员方法了

不是servlet了service方法也不会调用



```
在这两个方法里干自己想干的初始化工作和销毁工作。

初始化和销毁工作并不是这俩个方法做的，是tomcat或者其他容器做的，只是在初始化和销毁时会执行这两个方法而已，所以你能把需要的准备和结束业务放到这两个方法中。

我也在想这个问题 不过看了其他人的回答 好像有答案了 再结合我书里写的内容init():容器初始化Servlet时调用该方法  我的理解是初始化工作由Servlet容器完成 不需要我们在init()方法体中写 init()方法体可以写一些 初始化的时候希望额外执行的内容 注意此时Servlet对象已经被容器创建了 创建后才会执行init()方法destroy()方法同理 写与不写 Servlet都会被Servlet容器销毁 如果在销毁的时候想执行一些额外工作 可以写  注意 此时Servlet对象还没有被销毁 被销毁前会执行destroy方法总结一下 init()方法在Servlet容器创建Servlet对象后执行  而destroy方法在Servlet对象被销毁前执行 共同点是 只会被执行一次 因为Servlet的生命周期 就是初始化 运行 销毁 初始化和销毁在一个生命周期中只能执行一次

作者：中本聪
链接：https://www.zhihu.com/question/267184460/answer/1309604002
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```







# Servlet详解之两个init方法的作用

 转载

[IT祖师爷](https://blog.51cto.com/u_9597987)2021-08-18 11:01:11

***文章标签\*[java](https://blog.51cto.com/topic/java.html)[tomcat](https://blog.51cto.com/topic/tomcat.html)[初始化](https://blog.51cto.com/topic/chushihua.html)*****文章分类\*[Java](https://blog.51cto.com/nav/java)[编程语言](https://blog.51cto.com/nav/program)*****阅读数\**\*37\****

在Servlet中 javax.servlet.GenericServlet类 继承自java.lang.Object 实现了Serializable,，servlet ，ServletConfig 三个接口 被继承对象javax.servlet.http.HttpServlet

（这是我们常用的一个类）

 

但仔细看GenericServlet的API，会发现有两个init方法 void init() void init(ServletConfig config) 而在Servlet接口中只有一个void init(ServletConfig config) 方法 

在此做一下解释：

 

**先看一下官方说明：** void init() A convenience method which can be overridden so that there's no need to call super.init(config). 
void init(ServletConfig config) Called by the servlet container to indicate to a servlet that the servlet is being placed into service. 

 

**看一下这两个函数的源码：**GenericServlet.class

 

**[java]** view plain copy print?

1. <span style="font-size: 18px;"> 
2. </span> 

```java
 1.
```

**[java]** view plain copy print?

1. <span style="font-size: 18px;">public void init(ServletConfig config) throws ServletException { 
2.   this.config = config; 
3.   this.init(); 
4.   }</span> 

```java
public void init(ServletConfig config) throws ServletException {
	this.config = config;
	this.init();
    }1.2.3.4.
```

 

 

 

**[java]** view plain copy print?

1. <span style="font-size: 18px;"> 
2. </span> 

```java
 1.
```

**[java]** view plain copy print?

1. <span style="font-size: 18px;">public void init() throws ServletException { 
2.  
3.   }</span> 

```java
public void init() throws ServletException {

    }1.2.3.
```

 

 

可以看到有参的init方法又调用了无参的init方法，这是什么原因呢。 补充一句，Tomcat默认调用的是有参的。
如果你曾复写过init（）方法，你应该能看出些端详 如果我们在想调用init方法时执行一些操作，那怎么办呢，只要我们复写无参的init方法，tomcat在调用完有参的init方法时调用无参的init方法，这样两个操作都执行了。而且我们也不用写super.init();试想一下，如果没有无参的init方法，那我们复写有参的init方法时忘记了些super.init(config)方法，那么**config就不能初始化**了，而这样设计即使我们不调用super的方法，也不会出问题。何乐而不为呢！





```
为了获取servlet上下文(servletComtext)
```

![image-20220909204455542](assets/202209092044649.png)





```
这个之前讲过 应该重写init()方法，或者重写init(ServletConfig)但是要调用super.init(servletconfig);
```





```
出现问题的主要原因是，furitController已经不是servlet了，但是其父类竟然还是需要servlet
```

```
不懂得可以多看看GenericServlet类的源码
```

