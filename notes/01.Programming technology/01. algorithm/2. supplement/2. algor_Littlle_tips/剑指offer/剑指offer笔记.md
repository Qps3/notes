# 动态规划（简单）

## 10- II. 青蛙跳台阶问题

n阶台阶到达路径数 = n-1阶台阶到达路径数 + n-2阶台阶到达路径数

- n=0，sum = 1

- n=1，sum = 1

```java
class Solution {
    public int numWays(int n) {

        int a=1,b=1,sum;

        for(int i=0;i<n;i++){
            sum = (a+b)%1000000007;
            a=b;
            b=sum;


        }
        return a;


    }
}
```

