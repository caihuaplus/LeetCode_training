# 70. 爬楼梯

> 假设你正在爬楼梯。需要 n 阶你才能到达楼顶。
>
> 每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
>
> 注意：给定 n 是一个正整数。
>
> 示例 1：
> ```
> 输入： 2
> 输出： 2
> 解释： 有两种方法可以爬到楼顶。
> 1.  1 阶 + 1 阶
> 2.  2 阶
> ```
> 示例 2：
> ```
> 输入： 3
> 输出： 3
> 解释： 有三种方法可以爬到楼顶。
> 1.  1 阶 + 1 阶 + 1 阶
> 2.  1 阶 + 2 阶
> 3.  2 阶 + 1 阶
> ```

## 思路

| 台阶数 |几种方式 |
|:-: | :- |
| 1 | 1 (只能迈一步)|
| 2 | 2 (迈一步、或者迈两步) |
| 3 | 3 (第二阶台阶和第一阶台阶方式之和)|
| n | (n -1) + (n -1) |

## 解法

### 1.简单的递归

```java
public int climbStairs(int n) {
    return climb_Stairs(0, n);
}

private int climb_Stairs(int i, int n) {
    if (i > n ) return 0;
    if (i == n) return 1;
    return climb_Stairs(i + 1, n) + climb_Stairs(i + 2, n);
}
```

此种解法的时间复杂度是 2^n。

空间复杂度是O(n)。

### 2. 斐波那契数列

```java
public static int climbStair(int n) {
    if (n == 1) {
        return 1;
    }
    int first = 1;
    int second = 2;
    for (int i = 3; i <= n; i++) {
        int third = first + second;
        first = second;
        second = third;
    }
    return second;
}
```