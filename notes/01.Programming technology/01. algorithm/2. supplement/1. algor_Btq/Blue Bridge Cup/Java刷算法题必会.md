# Java刷算法题必会



## 基本概念

### 常用的包



<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br></pre></td><td class="code" id="code-0"><pre><span class="line"><span class="hljs-keyword">import</span> java.util.*;</span><br><span class="line"><span class="hljs-keyword">import</span> java.lang.*;</span><br></pre></td></tr></tbody></table>

  

### 数据类型、接口和类

- 接口：抽象方法的集合，但没有特定方法的实现
- 类：通过继承接口从而继承接口的抽象方法并针对不同的方法有自己的实现

| 接口 |    特点    |        常用类         |
| :--: | :--------: | :-------------------: |
| List | 元素不唯一 | LinkedList、ArrayList |
| Set  |  元素唯一  |   HashSet、TreeSet    |
| Map  |  键值映射  |   HashMap、TreeMap    |


### 常用类列表

该部分仅列出了在算法题中经常用到的类型与数据结构。

|           类           | 适用情况                                                     |
| :--------------------: | ------------------------------------------------------------ |
|       数组 Array       | 既定长度，可模拟pair、tuple等immutable容器                   |
|   动态数组 ArrayList   | 长度动态可改的灵活数组                                       |
|        栈 Stack        | LIFO后进先出                                                 |
|       队列 Queue       | FIFO先进先出                                                 |
|      集合 HashSet      | 当需要存储**非重复**元素或**去重**时，优先考虑该类型，但其不保证有序 |
|     哈希表 HashMap     | 实现O(1)的读取与写入键值对，但其不保证有序                   |
| 优先队列 PriorityQueue | 利用Heap进行排序性存值                                       |


### 基本变量类型 - Basic Variable Types

|  类型   | 备注                   |
| :-----: | ---------------------- |
|  byte   |                        |
|   int   | 范围 -2^31 to 2^31 – 1 |
|  long   | 范围 -2^63 to 2^63 – 1 |
|  float  |                        |
| double  |                        |
|  char   |                        |
| boolean |                        |
### 字符 - Char

|         用途         | 语法                                 | 备注         |
| :------------------: | ------------------------------------ | ------------ |
|         创建         | `char example = 'a'`                 | 注意是单引号 |
|         比较         | `example1 = example2`                |              |
| 判断是否为纯数字字母 | `Character.isLetterOrDigit(example)` |              |


### 字符串 - String

|              用途               | 语法                                      | 备注                  |
| :-----------------------------: | ----------------------------------------- | --------------------- |
|              创建               | `String example = "Hello"`                | 注意是双引号          |
|        根据char数组创建         | `String example = new String({'a', 'b'})` |                       |
| 重复已有字符串3次以创建新字符串 | `const newExample = example.repeat(3);`   |                       |
|            获取长度             | `example.length()`                        |                       |
|          获取第3位字符          | `example.charAt(3)`                       | 绝不能用\[\]          |
|       第3位字符的ascii码        | `(int) example.charAt(3)`                 |                       |
|     将ascii码66转为对应字符     | `(char) 66`                               | 字符+数字实现字符递增 |
|     字符’a’第一次出现的位置     | `example.indexOf('a')`                    |                       |
|    字符’a’最后一次出现的位置    | `example.lastIndexOf('a')`                |                       |
|  将字符串中的所有’a’替换为’b’   | `example.replace('a', 'b')`               |                       |
|     以空格为分界转换为数组      | `example.split(' ')`                      |                       |
|     将字符串转换为字符数组      | `example.toCharArray()`                   |                       |
|   提取前5位字符组成的子字符串   | `example.substring(0, 5)`                 |                       |
|      将字符串全部化为小写       | `example.toLowerCase()`                   |                       |
|      将字符串全部化为大写       | `example.toUpperCase()`                   |                       |
|       将字符串转化为整数        | `Integer.parseInt(example);`              |                       |
|       将整数转化为字符串        | `Integer.toString(example);`              |                       |
|     判断字符串是不是纯整数      | `Integer.parseInt(example);`              | 要用try-catch捕获异常 |
|     判断两个字符串是否一样      | `str1.equals(str2)`                       | 绝对不能用==          |


### 数字 & 数学 - Number & Math

|   用途   | 语法                | 备注 |
| :------: | ------------------- | ---- |
| 最大整数 | `Integer.MAX_VALUE` |      |
| 最小整数 | `Integer.MIN_VALUE` |      |

## 常用类实例

### 数组 - Array

Copy

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br><span class="line">5</span><br><span class="line">6</span><br><span class="line">7</span><br><span class="line">8</span><br><span class="line">9</span><br><span class="line">10</span><br><span class="line">11</span><br><span class="line">12</span><br><span class="line">13</span><br><span class="line">14</span><br><span class="line">15</span><br><span class="line">16</span><br><span class="line">17</span><br><span class="line">18</span><br><span class="line">19</span><br><span class="line">20</span><br></pre></td><td class="code" id="code-1"><pre><span class="line"><span class="hljs-keyword">int</span>[] example1 = <span class="hljs-keyword">new</span> <span class="hljs-keyword">int</span>[]{<span class="hljs-number">1</span>, <span class="hljs-number">2</span>, <span class="hljs-number">3</span>};  <span class="hljs-comment">//初始化int数组</span></span><br><span class="line"><span class="hljs-keyword">int</span>[] example2 = <span class="hljs-keyword">new</span> <span class="hljs-keyword">int</span>[<span class="hljs-number">3</span>];  <span class="hljs-comment">//初始化长度为3的int数组</span></span><br><span class="line"></span><br><span class="line">Arrays.fill(example2, <span class="hljs-number">0</span>);  <span class="hljs-comment">//将数组全部初始化为0</span></span><br><span class="line">example1[<span class="hljs-number">0</span>] = <span class="hljs-number">0</span>; <span class="hljs-comment">//修改第0位元素</span></span><br><span class="line">example1.length; <span class="hljs-comment">//数组长度（注意，不是方法！）</span></span><br><span class="line">Arrays.equals(example1, example2); <span class="hljs-comment">//比较两个数组</span></span><br><span class="line">Arrays.sort(example1); <span class="hljs-comment">//数组递增排序</span></span><br><span class="line">Arrays.sort(example1, Collections.reverseOrder()); <span class="hljs-comment">//数组递减排序</span></span><br><span class="line"></span><br><span class="line">String text = Arrays.toString(example1); <span class="hljs-comment">//将数组转化为字符串(需要导入java.util.*)</span></span><br><span class="line"></span><br><span class="line"><span class="hljs-comment">// 除了用普通index方法遍历外：</span></span><br><span class="line"><span class="hljs-keyword">for</span>(<span class="hljs-keyword">int</span> num: example1){</span><br><span class="line">    System.out.println(num);</span><br><span class="line">}</span><br><span class="line"></span><br><span class="line"><span class="hljs-comment">// 不声明且直接推入容器</span></span><br><span class="line">ArrayList&lt;<span class="hljs-keyword">int</span>[]&gt; arrList = <span class="hljs-keyword">new</span> ArrayList&lt;&gt;();</span><br><span class="line">arrList.add(<span class="hljs-keyword">new</span> <span class="hljs-keyword">int</span>[]{<span class="hljs-number">1</span>,<span class="hljs-number">2</span>});</span><br></pre></td></tr></tbody></table>

  

### 动态数组 - ArrayList

Copy

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br><span class="line">5</span><br><span class="line">6</span><br><span class="line">7</span><br><span class="line">8</span><br><span class="line">9</span><br><span class="line">10</span><br><span class="line">11</span><br><span class="line">12</span><br><span class="line">13</span><br><span class="line">14</span><br><span class="line">15</span><br><span class="line">16</span><br><span class="line">17</span><br><span class="line">18</span><br><span class="line">19</span><br><span class="line">20</span><br><span class="line">21</span><br><span class="line">22</span><br><span class="line">23</span><br></pre></td><td class="code" id="code-2"><pre><span class="line"><span class="hljs-keyword">import</span> java.util.*; </span><br><span class="line"></span><br><span class="line">ArrayList&lt;String&gt; example = <span class="hljs-keyword">new</span> ArrayList&lt;&gt;(); </span><br><span class="line">example.add(<span class="hljs-string">"a"</span>); <span class="hljs-comment">//向尾部添加元素</span></span><br><span class="line">example.add(<span class="hljs-string">"c"</span>); </span><br><span class="line">example.add(<span class="hljs-number">1</span>, <span class="hljs-string">"b"</span>); <span class="hljs-comment">//在第1位添加元素</span></span><br><span class="line">String val = example.get(<span class="hljs-number">0</span>); <span class="hljs-comment">//获取第0位元素</span></span><br><span class="line"><span class="hljs-keyword">int</span> firstPos = example.indexOf(<span class="hljs-string">"b"</span>); <span class="hljs-comment">//找到指定元素第一次出现的位置</span></span><br><span class="line"><span class="hljs-keyword">int</span> lastPos = example.lastIndexOf(<span class="hljs-string">"b"</span>); <span class="hljs-comment">//找到指定元素最后一次出现的位置</span></span><br><span class="line">List&lt;String&gt; sub_example = example.subList(<span class="hljs-number">0</span>, <span class="hljs-number">2</span>); <span class="hljs-comment">//获取子数组</span></span><br><span class="line"><span class="hljs-keyword">int</span> length = example.size(); <span class="hljs-comment">//得到数组长度</span></span><br><span class="line"></span><br><span class="line">example.remove(<span class="hljs-number">0</span>); <span class="hljs-comment">//删除第0位元素</span></span><br><span class="line">String converted = String.join(<span class="hljs-string">", "</span>, example); <span class="hljs-comment">//将数组转换为字符串并用', '分隔</span></span><br><span class="line">example.clear(); <span class="hljs-comment">//清空数组</span></span><br><span class="line"></span><br><span class="line"><span class="hljs-comment">// 除了用普通index方法遍历外：</span></span><br><span class="line"><span class="hljs-keyword">for</span>(String x: example){</span><br><span class="line">    System.out.println(x);</span><br><span class="line">}</span><br><span class="line"></span><br><span class="line"><span class="hljs-comment">//将ArrayList转化为Array，注意在括号中要声明存放结果的数据结构</span></span><br><span class="line">String[] arr = example.toArray(<span class="hljs-keyword">new</span> String[example.size()]);</span><br></pre></td></tr></tbody></table>

动态数组的排序默认用Collection中的sort方法，若要自定义则需要comparator。

> 自定义comparator的时候要注意计算时可能出现的数据溢出，建议用cast或者Object（像是Integer）来处理。

Copy

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br><span class="line">5</span><br><span class="line">6</span><br><span class="line">7</span><br><span class="line">8</span><br><span class="line">9</span><br><span class="line">10</span><br><span class="line">11</span><br><span class="line">12</span><br><span class="line">13</span><br><span class="line">14</span><br><span class="line">15</span><br><span class="line">16</span><br><span class="line">17</span><br><span class="line">18</span><br><span class="line">19</span><br><span class="line">20</span><br><span class="line">21</span><br><span class="line">22</span><br><span class="line">23</span><br><span class="line">24</span><br><span class="line">25</span><br><span class="line">26</span><br><span class="line">27</span><br><span class="line">28</span><br><span class="line">29</span><br></pre></td><td class="code" id="code-3"><pre><span class="line"><span class="hljs-comment">// 方法一：对数字或字符串进行常规排序</span></span><br><span class="line">ArrayList&lt;Integer&gt; example = <span class="hljs-keyword">new</span> ArrayList&lt;&gt;();</span><br><span class="line">Collections.sort(arraylist); <span class="hljs-comment">// 默认升序</span></span><br><span class="line">Collections.sort(al, Collections.reverseOrder()); <span class="hljs-comment">// 降序</span></span><br><span class="line"></span><br><span class="line"><span class="hljs-comment">// 方法二：根据列表中所含的对象的特定成员进行排序（也可以用在Linked List上）</span></span><br><span class="line"><span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">Employee</span> </span>{</span><br><span class="line">    <span class="hljs-keyword">int</span> id;</span><br><span class="line">    <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-title">Employee</span><span class="hljs-params">(<span class="hljs-keyword">int</span> id)</span></span>{</span><br><span class="line">        <span class="hljs-keyword">this</span>.id = id;</span><br><span class="line">    }</span><br><span class="line">}</span><br><span class="line"><span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">SortbyId</span> <span class="hljs-keyword">implements</span> <span class="hljs-title">Comparator</span>&lt;<span class="hljs-title">Employee</span>&gt; </span></span><br><span class="line"><span class="hljs-class"></span>{</span><br><span class="line">    <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">int</span> <span class="hljs-title">compare</span><span class="hljs-params">(Employee a, Employee b)</span></span>{ </span><br><span class="line">        <span class="hljs-comment">/**</span></span><br><span class="line"><span class="hljs-comment">         * 返回值为负数则a比b小（a在b前面），返回为正数则a大于b（a在b后面），返回为零意味着o1等于o2</span></span><br><span class="line"><span class="hljs-comment">         */</span></span><br><span class="line">        <span class="hljs-keyword">return</span> a.id - b.id; <span class="hljs-comment">// 倒序则改为：b.id-a.id</span></span><br><span class="line">    } </span><br><span class="line">} </span><br><span class="line"></span><br><span class="line">ArrayList&lt;Employee&gt; example = <span class="hljs-keyword">new</span> ArrayList&lt;&gt;();</span><br><span class="line">Collections.sort(example, <span class="hljs-keyword">new</span> SortbyId()); </span><br><span class="line"></span><br><span class="line"><span class="hljs-comment">//方法三：也可以用Lambda函数实现（Java 8即可）</span></span><br><span class="line">Collections.sort(example, (Employee a, Employee b) -&gt; {</span><br><span class="line">    <span class="hljs-keyword">return</span> a.id - b.id;</span><br><span class="line">});</span><br></pre></td></tr></tbody></table>

  

### 栈 - Stack

Copy

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br><span class="line">5</span><br><span class="line">6</span><br><span class="line">7</span><br><span class="line">8</span><br></pre></td><td class="code" id="code-4"><pre><span class="line"><span class="hljs-keyword">import</span> java.util.Stack;</span><br><span class="line"></span><br><span class="line">Stack&lt;String&gt; stack = <span class="hljs-keyword">new</span> Stack&lt;&gt;();</span><br><span class="line">stack.push(<span class="hljs-string">"a"</span>); <span class="hljs-comment">//推入元素</span></span><br><span class="line">String top = stack.peek(); <span class="hljs-comment">//查看栈首位元素</span></span><br><span class="line"><span class="hljs-keyword">int</span> length = stack.size(); <span class="hljs-comment">//得到栈高度</span></span><br><span class="line">String val = stack.pop(); <span class="hljs-comment">//出栈且返回该值</span></span><br><span class="line">stack.clear(); <span class="hljs-comment">//清除整个栈</span></span><br></pre></td></tr></tbody></table>

  

### 队列 - Queue

> 因避免捕获异常，不建议用add和remove操作

Copy

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br><span class="line">5</span><br><span class="line">6</span><br><span class="line">7</span><br><span class="line">8</span><br><span class="line">9</span><br></pre></td><td class="code" id="code-5"><pre><span class="line"><span class="hljs-keyword">import</span> java.util.LinkedList; </span><br><span class="line"><span class="hljs-keyword">import</span> java.util.Queue; </span><br><span class="line"></span><br><span class="line">Queue&lt;String&gt; queue = <span class="hljs-keyword">new</span> LinkedList&lt;&gt;(); </span><br><span class="line">queue.offer(<span class="hljs-string">"a"</span>); <span class="hljs-comment">//插入元素</span></span><br><span class="line">String top = queue.peek(); <span class="hljs-comment">//查看队列首位元素</span></span><br><span class="line"><span class="hljs-keyword">int</span> length = queue.size(); <span class="hljs-comment">//得到队列长度</span></span><br><span class="line">String val = queue.poll(); <span class="hljs-comment">//出队且返回该值</span></span><br><span class="line">queue.clear(); <span class="hljs-comment">//清除整个队列</span></span><br></pre></td></tr></tbody></table>

  

### 优先队列 - PriorityQueue

> 因避免捕获异常，不建议用add和remove操作

Copy

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br><span class="line">5</span><br><span class="line">6</span><br><span class="line">7</span><br><span class="line">8</span><br><span class="line">9</span><br><span class="line">10</span><br><span class="line">11</span><br><span class="line">12</span><br><span class="line">13</span><br><span class="line">14</span><br><span class="line">15</span><br><span class="line">16</span><br><span class="line">17</span><br></pre></td><td class="code" id="code-6"><pre><span class="line"><span class="hljs-keyword">import</span> java.util.PriorityQueue; </span><br><span class="line"></span><br><span class="line">PriorityQueue&lt;Integer&gt; pQueue = <span class="hljs-keyword">new</span> PriorityQueue&lt;&gt;(); </span><br><span class="line">PriorityQueue&lt;Integer&gt; pQueueInit = <span class="hljs-keyword">new</span> PriorityQueue&lt;&gt;(n, cmp); <span class="hljs-comment">//定义初始大小n，以及定义好的比较函数</span></span><br><span class="line">pQueue.offer(<span class="hljs-number">15</span>); </span><br><span class="line">pQueue.offer(<span class="hljs-number">5</span>); </span><br><span class="line"><span class="hljs-keyword">int</span> top = pQueue.peek(); <span class="hljs-comment">//查看优先队列首位元素，默认从小到大排序</span></span><br><span class="line"><span class="hljs-keyword">int</span> length = pQueue.size(); <span class="hljs-comment">//得到优先队列长度</span></span><br><span class="line"><span class="hljs-keyword">int</span> val = pQueue.poll(); <span class="hljs-comment">//出队且返回该值</span></span><br><span class="line"><span class="hljs-keyword">int</span>[] array = pQueue.toArray(); <span class="hljs-comment">//将优先队列转换为数组</span></span><br><span class="line"></span><br><span class="line"><span class="hljs-comment">/* 迭代器对集合进行遍历 */</span></span><br><span class="line">Iterator itr = pQueue.iterator();</span><br><span class="line"><span class="hljs-keyword">while</span> (itr.hasNext()) </span><br><span class="line">    System.out.println(itr.next()); </span><br><span class="line"></span><br><span class="line">pQueue.clear(); <span class="hljs-comment">//清除整个优先队列</span></span><br></pre></td></tr></tbody></table>

  

### 集合 - HashSet

Copy

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br><span class="line">5</span><br><span class="line">6</span><br><span class="line">7</span><br><span class="line">8</span><br><span class="line">9</span><br><span class="line">10</span><br><span class="line">11</span><br><span class="line">12</span><br><span class="line">13</span><br><span class="line">14</span><br><span class="line">15</span><br><span class="line">16</span><br><span class="line">17</span><br><span class="line">18</span><br><span class="line">19</span><br><span class="line">20</span><br><span class="line">21</span><br><span class="line">22</span><br><span class="line">23</span><br><span class="line">24</span><br><span class="line">25</span><br><span class="line">26</span><br><span class="line">27</span><br></pre></td><td class="code" id="code-7"><pre><span class="line"><span class="hljs-keyword">import</span> java.util.HashSet; </span><br><span class="line"></span><br><span class="line">HashSet&lt;String&gt; set = <span class="hljs-keyword">new</span> HashSet&lt;&gt;(); </span><br><span class="line"></span><br><span class="line">set.add(<span class="hljs-string">"a"</span>); <span class="hljs-comment">//增加元素</span></span><br><span class="line">set.add(<span class="hljs-string">"b"</span>); </span><br><span class="line">set.add(<span class="hljs-string">"c"</span>); </span><br><span class="line"></span><br><span class="line"><span class="hljs-keyword">boolean</span> exists = set.contains(<span class="hljs-string">"b"</span>); <span class="hljs-comment">//检查元素是否存在</span></span><br><span class="line"><span class="hljs-keyword">int</span> length = set.size(); <span class="hljs-comment">//查看集合大小</span></span><br><span class="line">set.remove(<span class="hljs-string">"b"</span>); <span class="hljs-comment">//删除元素</span></span><br><span class="line"></span><br><span class="line"><span class="hljs-comment">/* 循环对集合进行遍历 */</span></span><br><span class="line"><span class="hljs-keyword">for</span> (String ptr : set) {</span><br><span class="line">    System.out.println(ptr);</span><br><span class="line">}</span><br><span class="line"></span><br><span class="line"><span class="hljs-comment">/* 迭代器对集合进行遍历（不常用） */</span></span><br><span class="line">Iterator&lt;String&gt; i = h.iterator(); </span><br><span class="line"><span class="hljs-keyword">while</span> (i.hasNext()) </span><br><span class="line">    System.out.println(i.next()); </span><br><span class="line"></span><br><span class="line"><span class="hljs-comment">/* 对集合进行拷贝 */</span></span><br><span class="line">HashSet&lt;String&gt; cloned_set = <span class="hljs-keyword">new</span> HashSet&lt;&gt;(); </span><br><span class="line">cloned_set.addAll(set); </span><br><span class="line"></span><br><span class="line">set.clear(); <span class="hljs-comment">//清空集合</span></span><br></pre></td></tr></tbody></table>

  

### 哈希表 - HashMap

Copy

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br><span class="line">5</span><br><span class="line">6</span><br><span class="line">7</span><br><span class="line">8</span><br><span class="line">9</span><br><span class="line">10</span><br><span class="line">11</span><br><span class="line">12</span><br><span class="line">13</span><br><span class="line">14</span><br><span class="line">15</span><br><span class="line">16</span><br><span class="line">17</span><br><span class="line">18</span><br><span class="line">19</span><br><span class="line">20</span><br><span class="line">21</span><br><span class="line">22</span><br><span class="line">23</span><br><span class="line">24</span><br><span class="line">25</span><br><span class="line">26</span><br><span class="line">27</span><br><span class="line">28</span><br><span class="line">29</span><br></pre></td><td class="code" id="code-8"><pre><span class="line"><span class="hljs-keyword">import</span> java.util.HashMap;</span><br><span class="line"></span><br><span class="line">HashMap&lt;String, Integer&gt; map = <span class="hljs-keyword">new</span> HashMap&lt;&gt;(); </span><br><span class="line"></span><br><span class="line">map.put(<span class="hljs-string">"a"</span>, <span class="hljs-number">1</span>); <span class="hljs-comment">//增加/修改指定键值对</span></span><br><span class="line">map.put(<span class="hljs-string">"b"</span>, <span class="hljs-number">2</span>); </span><br><span class="line">map.put(<span class="hljs-string">"c"</span>, <span class="hljs-number">3</span>);</span><br><span class="line"></span><br><span class="line"><span class="hljs-keyword">int</span> val = map.get(<span class="hljs-string">"a"</span>); <span class="hljs-comment">//根据键获取值</span></span><br><span class="line"><span class="hljs-keyword">boolean</span> keyExists = map.containsKey(<span class="hljs-string">"c"</span>); <span class="hljs-comment">//判断指定键是否存在</span></span><br><span class="line"><span class="hljs-keyword">boolean</span> valExists = map.containsValue(<span class="hljs-number">2</span>); <span class="hljs-comment">//判断指定值是否存在</span></span><br><span class="line">map.remove(<span class="hljs-string">"b"</span>); <span class="hljs-comment">//删除指定键值对</span></span><br><span class="line"><span class="hljs-keyword">int</span> size = map.size(); <span class="hljs-comment">//得到哈希表的大小</span></span><br><span class="line"></span><br><span class="line"><span class="hljs-comment">/* 通过所有键遍历整个哈希表 */</span></span><br><span class="line"><span class="hljs-keyword">for</span> (String key : map.keySet()) {</span><br><span class="line">    System.out.println(map.get(key));</span><br><span class="line">}</span><br><span class="line"></span><br><span class="line"><span class="hljs-comment">/* 遍历哈希表中的所有值*/</span></span><br><span class="line"><span class="hljs-keyword">for</span> (<span class="hljs-keyword">int</span> value : map.values()) {</span><br><span class="line">    System.out.println(value);</span><br><span class="line">}</span><br><span class="line"></span><br><span class="line"><span class="hljs-comment">/* 构造浅拷贝 */</span></span><br><span class="line">HashMap&lt;Integer, String&gt; cloned_map = <span class="hljs-keyword">new</span> HashMap&lt;&gt;(); </span><br><span class="line">cloned_map.putAll(hash_map); </span><br><span class="line"></span><br><span class="line">map.clear(); <span class="hljs-comment">//清除整个哈希表</span></span><br></pre></td></tr></tbody></table>

  

## 指针

Java一律遵循pass-by-value原则。白话理解，基本类型的值就直接保存在变量中，引用类型的变量保存的都是实际内容所在的地址，从而传递到函数参数的情况不一样，前者是值、后者是对应的地址，但都为相同的副本。就后者而言，如果在函数内改变了原先副本的地址（指向新对象），原传入的参数还是不变，所以一切改动都不会影响原传入参数。

> this关键字指的是在当前对象的实例，涵盖对象中的成员变量或者方法。

Copy

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br><span class="line">5</span><br><span class="line">6</span><br><span class="line">7</span><br><span class="line">8</span><br><span class="line">9</span><br><span class="line">10</span><br><span class="line">11</span><br><span class="line">12</span><br><span class="line">13</span><br><span class="line">14</span><br><span class="line">15</span><br><span class="line">16</span><br><span class="line">17</span><br><span class="line">18</span><br><span class="line">19</span><br></pre></td><td class="code" id="code-9"><pre><span class="line"><span class="hljs-comment">// 情况一：基本类型</span></span><br><span class="line"><span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-title">double</span><span class="hljs-params">(<span class="hljs-keyword">int</span> i)</span> </span>{ </span><br><span class="line">    i *= <span class="hljs-number">2</span>;</span><br><span class="line">} </span><br><span class="line"></span><br><span class="line"><span class="hljs-comment">// 情况二：引用类型（对象）</span></span><br><span class="line"><span class="hljs-comment">/* 如果该函数中重新将参数变量指向另一个对象，则main中的结果不变 */</span></span><br><span class="line"><span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-title">change</span><span class="hljs-params">(<span class="hljs-keyword">int</span>[] arr)</span></span>{</span><br><span class="line">    arr[<span class="hljs-number">0</span>] = <span class="hljs-number">100</span>;</span><br><span class="line">}</span><br><span class="line"></span><br><span class="line"><span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">static</span> <span class="hljs-keyword">void</span> <span class="hljs-title">main</span><span class="hljs-params">(String[] args)</span> </span>{ </span><br><span class="line">    <span class="hljs-keyword">int</span>[] arr = { <span class="hljs-number">1</span>, <span class="hljs-number">2</span>, <span class="hljs-number">3</span>, <span class="hljs-number">4</span>, <span class="hljs-number">5</span> };</span><br><span class="line">    <span class="hljs-keyword">int</span> i = <span class="hljs-number">100</span>;</span><br><span class="line">    <span class="hljs-keyword">double</span>(i);</span><br><span class="line">    change(arr);</span><br><span class="line">    System.out.println(i); <span class="hljs-comment">// 100，不变</span></span><br><span class="line">    System.out.println(count[<span class="hljs-number">0</span>]); <span class="hljs-comment">// 100，变了</span></span><br><span class="line">}</span><br></pre></td></tr></tbody></table>


所以对有时需要对参数传入的对象进行拷贝来保存特定的结果。深拷贝的方法需要手动实现，可以使用递归或是序列化的方法。这里只考虑容器单层存储基本数据类型和String（因其为final类型不可改）的浅拷贝情况，具体的方法和实现如下：

Copy

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br><span class="line">5</span><br><span class="line">6</span><br><span class="line">7</span><br><span class="line">8</span><br><span class="line">9</span><br><span class="line">10</span><br><span class="line">11</span><br></pre></td><td class="code" id="code-10"><pre><span class="line"><span class="hljs-comment">// 数组</span></span><br><span class="line">String[] a1 = {<span class="hljs-string">"a1"</span>, <span class="hljs-string">"a2"</span>};</span><br><span class="line">String[] a2 = a1.clone();</span><br><span class="line"></span><br><span class="line"><span class="hljs-comment">// 动态数组</span></span><br><span class="line">List&lt;String&gt; newList = <span class="hljs-keyword">new</span> ArrayList&lt;&gt;(oldList); <span class="hljs-comment">//第一种 </span></span><br><span class="line">newList.addAll(oldList); <span class="hljs-comment">//第二种</span></span><br><span class="line"></span><br><span class="line"><span class="hljs-comment">// 哈希表</span></span><br><span class="line">Map&lt;String, Integer&gt; newMap = <span class="hljs-keyword">new</span> HashMap&lt;&gt;(oldMap); <span class="hljs-comment">//第一种 </span></span><br><span class="line">Map&lt;String, Integer&gt; newMap = oldMap.clone(); <span class="hljs-comment">//第二种</span></span><br></pre></td></tr></tbody></table>

[#面试](https://www.kwanstyle.com/tags/%E9%9D%A2%E8%AF%95/) [#算法，刷题，干货](https://www.kwanstyle.com/tags/%E7%AE%97%E6%B3%95%EF%BC%8C%E5%88%B7%E9%A2%98%EF%BC%8C%E5%B9%B2%E8%B4%A7/) [#后端](https://www.kwanstyle.com/tags/%E5%90%8E%E7%AB%AF/)