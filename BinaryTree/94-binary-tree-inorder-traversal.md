# 94. 二叉树的中序遍历

> 给定一个二叉树，返回它的中序 遍历。
>
> 示例:
> 
> ```
> 输入: [1,null,2,3]
>    1
>     \
>      2
>     /
>    3
> 
> 输出: [1,3,2]
> ```
> 
> 进阶: 递归算法很简单，你可以通过迭代算法完成吗？

## 解题思路

### 1. 递归

首先递归左子树，然后左子树为null时，添加，再递归右子树。

```java
public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> res = new ArrayList<>();
    helper(root, res);
    return res;
}

private void helper(TreeNode root, List<Integer> res) {
    if (root != null) {
        if (root.left != null) {
            helper(root.left, res);
        }
        res.add(root.val);
        if (root.right != null) {
            helper(root.right, res);
        }
    }
}
```

### 2. 基于栈的遍历

本地维护一个栈 stack，从开始存储左节点。

```java
public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> res = new ArrayList<>();
    Stack<TreeNode> stack = new Stack<>();
    TreeNode curr = root;
    while (curr != null || !stack.isEmpty()) {
        while (curr != null) {
            stack.push(curr);
            curr = curr.left;
        }
        curr = stack.pop();
        res.add(curr.val);
        curr = curr.right;
    }

    return res;
}
```