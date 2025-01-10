# 算法题必备Tips（Java）

[![](https://p3-passport.byteimg.com/img/user-avatar/6dba765e507d4ec9c5e6d63fa39318d1~100x100.awebp)](https://juejin.cn/user/3078287578114766)



> 在我们刷题中，每个编程语言都有自己便捷的一处，例如javascript使用Set对数组去重，就一句话搞定，比Java爽之又爽。那么，本文整理了Java在实际刷题中常使用的一些方法。

### 1\. 检验字符能否转化为数字

```java
String s1 = "abc";
String s2 = "3";
if(s1.charAt(0) < 'a' ){ // 利用ASCII编码大小与‘a’的比较结果
    System.out.pringln("s1能转化为数字");
}else{
    System.out.pringln("s1不能转化为数字");
} 
复制代码
```

#### [例题：2042. 检查句子中的数字是否递增](https://link.juejin.cn/?target=https%3A%2F%2Fleetcode-cn.com%2Fproblems%2Fcheck-if-numbers-are-ascending-in-a-sentence%2F "https://leetcode-cn.com/problems/check-if-numbers-are-ascending-in-a-sentence/")

### 2\. int与String的互相转化

#### int -> String

```java
int num = 88; 

// 方式一： 
String s1 = "" + num; 

// 方式二： 
String s2 = String.valueOf(num); 
复制代码
```

#### String -> int

```java
String s = "88";

// 方式一：（推荐）
int num1 = Integer.parseInt(s);

// 方式二：
Integer i1 = Integer.valuOf(s);
int num2 = i1.intValue(i1);
复制代码
```

### 3\. 数组相关

#### 3.1 数组排序

```java
// arr: [3, 4, 2, 1, 5]
// 由小到大
public void sort(int[] arr){
    Arrays.sort(arr);
}
// [1,2,3,4,5]

// 由大到小
// 注意，反转的话，要把数组int[]转化为Intger[]
public void sort(Integer[] arr){
    Arrays.sort(arr,Collections.reverseOrder());
}
复制代码
```

#### 3.2 数组去重

```java
// 方法一：HashSet
public Integer[] removeDuplicates(int[] arr){
    HashSet<Integer> hashSet = new HashSet<>();
    for(int i=0; i<arr.length; i++){
        hashSet.add(arr[i]);
    }
    return hashSet.toArray(new Integer[hashSet.size()]);
}
// 方法二：ArrayList
public Integer[] removeDuplicates(int[] arr){
    List<Integer> list = new ArrayList<>();
    for(int i=0; i<arr.length; i++){
        if(!list.contains(arr[i])){
            list.add(arr[i]);
        }
    }
    return list.toArray(new Integer[list.size()]);
}
复制代码
```

#### 3.3 数组反转

使用**Colleactions.reverse()** 方法

```java
public void reverse(Integer[] arr){
    Collections.reverse(Arrays.asList(arr));
    System.out.println(Arrays.asList(arr));
}
复制代码
```

#### 3.4 数组切片

`Arrays.copyOfRange(int[] arr,int from,int to);`

```java
public int[] split(int[] arr){
    int[] ints = Arrays.copyOfRange(arr, 1, 2);
    return ints;
}
复制代码
```

数组的切片在很多题中，并不复制出子组，而是使用两个指针去指出头和尾，例如滑动窗口。

#### 3.5 数组元素相等

```java
int[] arr1 = new int[]{1,3,5,6};
int[] arr2 = new int[]{5,6,3,1};
Arrays.equals(arr1, arr2) // true
复制代码
```

### 4\. String字符串相关

#### 4.1 获取index处的字符

`char charAt(int index)`

很多题没必要上来就把String用split分割为String数组，可以考虑用chaAt()获取某处的字符char。

#### 4.2 字符串分割split

`String[] split(String regex, int limit)`

```java
String str = new String("Hello-world-juejin"); 
String[] arr = str.split("-");

// [Hello,world,juejin]
复制代码
```

#### 4.3 判断两个字符串相等

不要用==，使用`boolean equals(Object anObject)`

```java
String str1 = "hello";
String str2 = "hello";
String str3 = new String("hello");

System.out(str1 == str2); // true
System.out(str1 == str3); // false
System.out(str1.equals(str3)); // true 
复制代码
```

### 5\. 优先队列PriorityQueue

Java中的PriorityQueue，默认是小根堆。

大根堆实现：

```java
        PriorityQueue<Integer> pq = new PriorityQueue<Integer>(new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2-o1;
            }
        });
复制代码
```

#### [例题：239. 滑动窗口最大值](https://link.juejin.cn/?target=https%3A%2F%2Fleetcode-cn.com%2Fproblems%2Fsliding-window-maximum%2F "https://leetcode-cn.com/problems/sliding-window-maximum/")

### 6\. 随机数

随机生成在某个范围的随机整数

```java
Random random = new Random();
int ran = random.nextInt(max)%(max-min+1)+min;

// 生成一个[5,30]的随机数
Random random = new Random();
int num = random.nextInt(30)%(30-5+1)+5;
复制代码
```

