# 283. 移动零

> 题目描述
> 
> 给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。
>
> 示例:
>
> 输入: [0,1,0,3,12]
> 输出: [1,3,12,0,0]
> 说明:
>
> 必须在原数组上操作，不能拷贝额外的数组。
> 尽量减少操作次数。

## 解题思路

将 0 移动到数组末尾，也就是将非零元素移动到数组的前面。

可以定义一个计数变量 count，记录当前 0 元素所在的位置，然后遇到非 0 元素后，与之交换，然后 count++。

### 代码实现

``` java
// 方法 1
// 定义一个计数变量，使用两个 for 循环实现移动
// 第一个 for 循环中只进行元素的赋值，比如[0,1,2,0,3] - for - > [1,2,3,0,3]，
// 赋值完成后，count 指向 index = 2 的元素 3;
// 第二个 for 循环将 count 后续的元素值改为零
public void moveZeroes(int[] nums) {
  if (nums == null || nums.length == 0) return;
  int count = 0;
  for (int i = 0; i < nums.length; i++) {
      if (nums[i] == 0) {
          count++;
      } else {
          nums[i - count] = nums[i];
      }
  }

  for (int i = nums.length - count; i < nums.length; i++){
      nums[i] = 0;
  }
}

```

``` java
// 方法 2
// 一层 for 循环，当 i != 0 时，进行元素的交换赋值
public void moveZeroes(int[] nums) {
  if (nums == null || nums.length == 0) return;
  int count = 0;
  for (int i = 0; i < nums.length; i++) {
      if (nums[i] != 0) {
          int temp = nums[count];
          nums[count++] = nums[i];
          nums[i] = temp;
      }
  }
}

```

```java
// 方法三
// 对方法 2 的一个优化，加入了 index 与 count 不一致时，在进行数据的交换。
public void moveZeroes(int[] nums) {
    if (nums == null || nums.length == 0) return;
    int count = 0;
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] != 0) {
            if (count != i) {
                nums[count] = nums[i];
                nums[i] = 0;
            }
            count++;
        }
    }
}
```

