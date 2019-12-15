# 20. 有效的括号

> 给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

> 有效字符串需满足：
> 
> 左括号必须用相同类型的右括号闭合。
> 左括号必须以正确的顺序闭合。
> 注意空字符串可被认为是有效字符串。
> 
> 示例 1:
> ```
> 输入: "()"
> 输出: true
> ```
> 示例 2:
> ```
> 输入: "()[]{}"
> 输出: true
> ```
> 示例 3:
> ```
> 输入: "(]"
> 输出: false
> ```
> 示例 4:
> ```
> 输入: "([)]"
> 输出: false
> ```
> 示例 5:
> ```
> 输入: "{[]}"
> 输出: true
> ```

## 解法思路

### 1. 暴力破解

不断的 replace 匹配的元素，将匹配的元素替换为“”，最后判断字符串是否为空。

```java
public boolean isValid(String s) {
    while (s.contains("()") || s.contains("[]") || s.contains("{}")) {
        if (s.contains("()")) s = s.replace("()","");
        if (s.contains("[]")) s = s.replace("[]","");
        if (s.contains("{}")) s = s.replace("{}","");
    }
    return s.isEmpty();
}
```

### 2. 栈 stack

1. 初始化一个栈 S。
2. 依次处理表达式的每一个括号
3. 如果遇到开括号 `[、(、{` 将其推入栈中
4. 如果遇到闭括号 `]、)、}` 检查站内有无对应的开阔好，有就弹出继续处理，没有就说明表达式是无效的。
5. 如果最后，栈内仍有元素，说明有未匹配的括号，表达式无效。


```java
private Map<Character,Character> map;
public Solution() {
    map = new HashMap<>();
    map.put(')', '(');
    map.put(']', '[');
    map.put('}', '{');
}

public boolean isValid(String s) {
    Stack<Character> stack = new Stack<>();
    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);
        if (map.containsKey(c)) {
            // 如果栈内有对应的括号，出栈
            // 获取堆栈的顶部元素。如果堆栈为空，则将虚拟值设置为“＃”
            char topElement = stack.isEmpty() ? '#' : stack.pop();
            // 如果此括号的映射与堆栈的top元素不匹配，则返回false。
            if (topElement != map.get(c)) {
                return false;
            }
        } else {
            stack.push(c);
        }
    }
    return stack.isEmpty();
}

```

### 3. 优化，去掉方法 2 的 map

在遇到开括号 `[、(、{` 时，将对应的闭括号 `]、)、}`  将其推入栈中

后续遇到闭括号时，只需要判断 stack.pop() 与它是否相等即可。

```java
 public boolean isValid2(String s) {
  Stack<Character> stack = new Stack<>();
  for (int i = 0; i < s.length(); i++) {
      char c = s.charAt(i);
      if (c == '(')
          stack.push(')');
      else if (c == '[')
          stack.push(']');
      else if (c == '{')
          stack.push('}');
      else if (stack.isEmpty() || stack.pop() != c) {
          return false;
      }
  }
  return stack.isEmpty();
}

```