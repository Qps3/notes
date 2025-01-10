

# Java刷题常用语法总结





#### 输入输出

```java
Scanner in=new Scann(System.in);
int n=in.nextInt();//读取单个字符；
String s=in.nextLine();//读取整行输入；
//优化：
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
String n=inBufferedReader.readLine();
```

#### 类型转换

```java
//Stinrg类型转换成int；其他同理
Integer.valueOf(string);
Integer.parseInt(string);
```

#### list 转换为int 数组

list转换为in\[\]\[\]

```java
List<int[]> list = new ArrayList<>();
list.toArray(new int[list.size()][2])
 List<Integer> temp =new ArrayList();
        temp.add(0, 12);
        temp.add(0, 122);
```

#### ArrayList 方法

```java
List<Integer> res=new ArrayList();
res.clear();//清空元素内容。
Collections.reverse(res) //将动态数组元素内容逆序。

```

#### 最大值最小值

```java
max = Integer.MAX_VALUE;
min = Integer.MIN_VALUE;
```

#### String

```
String s="xxxx";
char[]ch=s.toCharArray();
s.subString(i,j);//截取下标从i到j-1;
String[]ss=s.split("regex");
```

#### 集合

##### HashMap

```java
//HashMap
Map<String,String> map = new HashMap<>();
map.put("key","value");
map.getOrDefault("key","default");//if(map.containsKey("key")) return "value" else return "default"
map.get("key"); 
map.containsKey("key");//是否包含指定key；
map.putIfAbsent("key","value"); //存在当前key,就用当前key对应的value;否则使用参数中的value；
//集合遍历
//推荐
for(Map.Entry<String,String> entry : map.entrySet()){
    entry.getKey();
    entry.getValue();
} 
 // 推荐
map.forEach((k, v) -> System.out.println(k + " : " + (v + 10))); 
// for
Map<Integer, Integer> map = new HashMap<Integer, Integer>();
for (Integer key : map.keySet()) {
    Integer value = map.get(key);
    System.out.println("Key = " + key + ", Value = " + value);
  }
// 使用Iterator迭代器
        Iterator hmIterator = map.entrySet().iterator(); 

  while (hmIterator.hasNext()) { 
            Map.Entry mapElement = (Map.Entry)hmIterator.next(); 
            int marks = ((int)mapElement.getValue() + 10); 
            System.out.println(mapElement.getKey() + " : " + marks); 
        } 

```

##### 队列

```java
Queue<Integer> q = new LinkedList<>(); 
 q.peek();
 q.poll();
 q.size();
 q.poll()当没有队列元素；出队会返回null;
    //优先级遍历
PriorityQueue pQueue = new riorityQueue(); 
Iterator itr = pQueue.iterator(); 
while (itr.hasNext()) 
    System.out.println(itr.next()); 
```

#### 按照指定位置排序

```java
//一种简单做法是通过Arrays.sort()传入新的Compartor对象；来实现按照指定序列排序；比如我们想实现二维数组，按照第一列升序，第二列降序
  Arrays.sort(arr, new Comparator<int[]>()
         {
             public int compare(int[] a, int[] b) {
                 return a[0] == b[0] ?
                     b[1] - a[1] : a[0] - b[0];
             }
         });
```

还可以使用lambda表达；推荐  
按照arr\[i\]\[0\]升序排序，然后再按照arr\[i\]\[1\]升序排序。

```java
      Arrays.sort(arr,(int[]a, int[]b)->{
              return  b[0]==a[0]?a[1]-b[1]:a[0]-b[0];
          });

```

#### 输出

```java
System.out.println(String.format("%.6f",((s-1)/n)*100));//保留6位小数 四舍五入

```

卡特兰数：  

应用：计算栈的合法出栈数量；不同的二叉搜索树  
**字符串加法**  
应用Integer.toBinaryString( value)方法将10进制转成成2进制字符串；

```java
  public static void main(String[] args){
        Main m=new Main();
        String a="110101";
        String b="1010110";
        String sum=m.add(a,b);
        System.out.println(sum);
    }
    public String add(String a,String b){
        int sum=Integer.valueOf(a,2)+Integer.valueOf(b,2);
        return Integer.toBinaryString(sum);
    }
```

**向上取整、向下取整**  
java m/n向下取整 m+n-1/n向上取整  
**数组填充制定数字**  
Arrays.fill(nums,number)填充数组指定值  
\*\* 求最大公约数\*\*

```java
public static int gcd(int m, int n) {
    return n == 0 ? m : gcd(n, m % n);
}
```

## 使用javaStream 求和

```java
 int sumA = Arrays.stream(A).sum();
 int sumB = Arrays.stream(B).sum();
```

### 优先级队列构造

```java
  PriorityQueue<Integer> minQueue = new PriorityQueue<>(Comparator.naturalOrder());
  PriorityQueue<Integer> maxQueue = new PriorityQueue<>(Comparator.reverseOrder());
```