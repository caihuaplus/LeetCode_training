# 11.盛最多水的容器

> 题目描述
>
> 给定 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。
>
> 说明：你不能倾斜容器，且 n 的值至少为 2。
>
> ![img](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/07/25/question_11.jpg)
>
> 图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。
> 
> 示例:
>
> ```
> 输入: [1,8,6,2,5,4,8,3,7]
> 输出: 49
> ```

## 解题思路

### 1.暴力破解

使用双层 for 循环遍历，对每对可能出现的线段组合求面积，并找出这些情况之下的最大面积。

```java
public static int maxArea(int[] height) {
    if (height == null || height.length == 0) return 0;
    int max = 0;
    for (int i = 0; i < height.length - 1; i++) {
        for (int j = i + 1; j < height.length; j++) {
            int area = (j - i) * Math.min(height[i], height[j]);
            max = Math.max(max, area);
        }
    }
    return max;
}
```

### 2.双指针法

思路是：两线段之间形成的区域总是会受到其中较短那条长度的限制。此外，两线段距离越远，得到的面积就越大。
定义两个指针，一个放在开始，一个置于末尾。使用 max 存储最大的面积
在每一步中，获取存储指针之间的面积，更新 max，并且将较短的指针向较长指针的方向移动。
******** 注意 *******
为什么移动较矮的指针，而不移动较高的指针呢？
在每次移动时，我们可以选择移动高指针或者矮指针，但由于矮指针限制了矩形的高度。
因此若移动高指针时，矩形的高度不变，单宽度会缩减，由此移动高指针不会带来面积的上升，所以选择移动矮的指针。

```java
public static int maxArea(int[] height) {
    int max = 0;
    for (int i = 0, j = height.length - 1; i < j; ) {
        int minHeight = height[j] > height[i] ? height[i ++] : height[j --];
        max = Math.max(max, (j - i + 1) * minHeight);
    }
    return max;
}
```