# 155. 最小栈

>设计一个支持 push，pop，top 操作，并能在常数时间内检索到最小元素的栈。
>
>push(x) -- 将元素 x 推入栈中。
>
>pop() -- 删除栈顶的元素。
>
>top() -- 获取栈顶元素。
>
>getMin() -- 检索栈中的最小元素。
>
>示例:
>
>```
>MinStack minStack = new MinStack();
>minStack.push(-2);
>minStack.push(0);
>minStack.push(-3);
>minStack.getMin();   --> 返回 -3.
>minStack.pop();
>minStack.top();      --> 返回 0.
>minStack.getMin();   --> 返回 -2.
>```


## 解法思路

思想：“以空间换时间”。

### 1. 同步辅助栈

辅助栈和数据栈同步

特点：编码简单，不用考虑一些边界情况，就有一点不好：辅助栈可能会存一些“不必要”的元素。

```java

class MinStack {
    // 数据栈
    private Stack<Integer> data;
    // 辅助栈
    private Stack<Integer> helper;

    /** initialize your data structure here. */
    public MinStack() {
        data = new Stack<>();
        helper = new Stack<>();
    }

    public void push(int x) {
        // 添加时，都添加
        data.add(x);
        if (helper.isEmpty() || helper.peek() >= x) {
            helper.add(x);
        } else {
            helper.add(helper.peek());
        }

    }

    public void pop() {
        if (!data.isEmpty()) {
            data.pop();
            helper.pop();
        }
    }

    public int top() {
        if (data != null) {
           return data.peek();
        }
        throw new RuntimeException("栈中元素为空");
    }

    public int getMin() {
        if (helper != null) {
            return helper.peek();
        }
        throw new RuntimeException("栈中元素为空");
    }
}

```

时间复杂度：O(1)，“出栈”、“入栈”、“查看栈顶元素”的操作不论数据规模多大，都只是有限个步骤，因此时间复杂度是：O(1)。
空间复杂度：O(N)，这里 N 是读出的数据的个数。

### 2. 不同步辅助栈

辅助栈和数据栈不同步

关键 1：辅助栈的元素空的时候，必须放入新进来的数

关键 2：新来的数小于或者等于辅助栈栈顶元素的时候，才放入（特别注意这里等于要考虑进去）

关键 3：出栈的时候，辅助栈的栈顶元素等于数据栈的栈顶元素，才出栈，即"出栈保持同步"就可以了

```java
class MinStack {
    // 数据栈
    private Stack<Integer> data;
    // 辅助栈
    private Stack<Integer> helper;

    /** initialize your data structure here. */
    public MinStack() {
        data = new Stack<>();
        helper = new Stack<>();
    }

    public void push(int x) {
        data.add(x);
        // 辅助栈只在必要时再添加。
        if (helper.isEmpty() || helper.peek() >= x) {
            helper.add(x);
        }
    }

    public void pop() {
        // 关键 3：data 一定得 pop()
        if (!data.isEmpty()) {
            // 注意：声明成 int 类型，这里完成了自动拆箱，从 Integer 转成了 int，因此下面的比较可以使用 "==" 运算符
            // 如果把 top 变量声明成 Integer 类型，下面的比较就得使用 equals 方法
            int top = data.pop();
            if (top == helper.peek()) {
                helper.pop();
            }
        }
    }

    public int top() {
        if (data != null) {
           return data.peek();
        }
        throw new RuntimeException("栈中元素为空");
    }

    public int getMin() {
        if (helper != null) {
            return helper.peek();
        }
        throw new RuntimeException("栈中元素为空");
    }
}
```