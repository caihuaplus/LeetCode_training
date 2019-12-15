# 189. 旋转数组

> 给定一个数组，将数组中的元素向右移动 k 个位置，其中 k 是非负数。
>
> 示例 1:
>
>```
>输入: [1,2,3,4,5,6,7] 和 k = 3
>输出: [5,6,7,1,2,3,4]
>解释:
>向右旋转 1 步: [7,1,2,3,4,5,6]
>向右旋转 2 步: [6,7,1,2,3,4,5]
>向右旋转 3 步: [5,6,7,1,2,3,4]
>```
>
>示例 2:
>
>```
>输入: [-1,-100,3,99] 和 k = 2
>输出: [3,99,-1,-100]
>解释: 
>向右旋转 1 步: [99,-1,-100,3]
>向右旋转 2 步: [3,99,-1,-100]
>```
>
>说明:
>
>尽可能想出更多的解决方案，至少有三种不同的方法可以解决这个问题。
>
>要求使用空间复杂度为 O(1) 的 原地 算法。

## 解法思路

### 1. 暴力破解

旋转 k 次，每次将数组旋转 1 个元素。

复杂度分析

时间复杂度：O(n*k) 。每个元素都被移动 1 步（O(n)） k次（O(k)）

空间复杂度：O(1)，没有使用额外的空间

```java
public void rotate(int[] nums, int k) {
    // 暴力解法
    int temp, previous;
    // 旋转 k 次
    for (int i = 0; i < k; i++) {
        // 获取当前旋转
        previous = nums[nums.length - 1];
        for (int j = 0; j < nums.length; j++) {
            temp = nums[j];
            nums[j] = previous;
            previous = temp;
        }
    }
}
```

### 2. 使用额外的数组

使用额外的一个数组存储正确的元素，然后再将它 copy 到数组中。

复杂度分析：

时间复杂度： O(n)。将数字放到新的数组中需要一遍遍历，另一边来把新数组的元素拷贝回原数组。

空间复杂度： O(n)。另一个数组需要原数组长度的空间。

```java
public void rotate(int[] nums, int k) {
    int[] copy =  new int[nums.length];
    for (int i = 0; i < nums.length; i++) {
        // 当前值 + k 对长度取余
        // nums = [1,2,3,4,5,6,7] k = 3
        // copy[[0 + 3] % 7] = copy[3] = 1
        // copy[[1 + 3] % 7] = copy[4] = 2
        // copy[[2 + 3] % 7] = copy[5] = 3
        // copy[[3 + 3] % 7] = copy[6] = 4
        // copy[[4 + 3] % 7] = copy[0] = 5
        // copy[[5 + 3] % 7] = copy[1] = 6
        // copy[[6 + 3] % 7] = copy[2] = 7
        copy[(i + k) % nums.length] = nums[i];
    }
    for (int i = 0; i < copy.length; i++) {
        nums[i] = copy[i];
    }
}
```

### 3. 反转

当我们旋转数组 k 次，k%n 个尾部元素会被移动到头部，剩下的元素会被向后移动。

我们首先将所有元素反转。然后反转前 k 个元素，再反转后面 n-k 个元素，就能得到想要的结果。

```
原始数组                  : 1 2 3 4 5 6 7
反转所有数字后             : 7 6 5 4 3 2 1
反转前 k 个数字后          : 5 6 7 4 3 2 1
反转后 n-k 个数字后        : 5 6 7 1 2 3 4 --> 结果
```

时间复杂度

时间复杂度：O(n)。 n 个元素被反转了总共 k 次。

空间复杂度：O(1)。 没有使用额外的空间。

```java
public void rotate(int[] nums, int k) {
    k %= nums.length;
    // 反转全部
    reverses(nums, 0, nums.length - 1);
    // 反转前 k 个
    reverses(nums, 0, k - 1);
    // 反转后 n-k 个
    reverses(nums, k, nums.length - 1);
}

private void reverses(int[] nums, int start, int end) {
    while (start < end) {
        int temp = nums[start];
        nums[start] = nums[end];
        nums[end] = temp;
        start++;
        end--;
    }
}
```

### 4. 使用环状替换（暂时没完全懂）

如果直接将每一个数字放到它最后的位置，就会遗失掉原来的元素。因此需要需要一个变量 temp 来存储替换的数字。

```
把元素看做同学，把下标看做座位，大家换座位。第一个同学离开座位去第k+1个座位，第k+1个座位的同学被挤出去了，他就去坐他后k个座位，如此反复。但是会出现一种情况，就是其中一个同学被挤开之后，坐到了第一个同学的位置（空位置，没人被挤出来），但是此时还有人没有调换位置，这样就顺着让第二个同学换位置。 那么什么时候就可以保证每个同学都换完了呢？n个同学，换n次，所以用一个count来计数即可。
```

复杂度分析

时间复杂度：O(n) 。只遍历了每个元素一次。

空间复杂度：O(1)。使用了常数个额外空间。

```java
public void rotate4(int[] nums, int k) {
  // 定义一个 count 变量用来计数
  int count = 0;
  for(int start = 0; count < nums.length; start++) {
      // 当前的指针位置
      int current = start;
      int prev = nums[start];
      do {
        int next = (current + k) % nums.length;
        int temp = nums[next];
        nums[next] = prev;
        prev = temp;
        current = next;
        count++;
      } while(start != current);
  }
}
```