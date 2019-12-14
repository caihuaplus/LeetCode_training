# 1. 两数之和

> 给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。
> 
> 你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。
> 
> 示例:
>```
> 给定 nums = [2, 7, 11, 15], target = 9
>
> 因为 nums[0] + nums[1] = 2 + 7 = 9
> 所以返回 [0, 1]
>```

## 解法

### 1. 暴力破解

使用双重 for 循环判断。

```java
private static int[] twoSum(int[] nums, int target) {
    for (int i = 0; i < nums.length - 1; i++) {
        for (int j = i + 1; j < nums.length; j++) {
            if (nums[i] + nums[j] == target) {
                return new int[]{i,j};
            }
        }
    }
    return new int[]{};
}
```

### 2. 哈希表

创建一个 HashMap 存储，通过一个 for 循环，每一次先判断目标 target - i 是否已经在 map 中，如果在就返回，不在就将当前值存储到 map 中，继续下一次循环。

```java
private static int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> hashMap = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        if (hashMap.containsKey(target - nums[i])) {
            return new int[]{hashMap.get(target - nums[i]), i};
        }
        hashMap.put(nums[i], i);
    }
    throw new IllegalArgumentException("No two sum Solution");
}
```