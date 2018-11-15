# 树的遍历
## 遍历的种类

### DFS
适用于中序， 先序和后序三种方式遍历树. 需要掌握递归的解法和非递归(用一个栈手动模拟递归)的解法

#### 递归解法 (preorder)
```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        helper(res, root);
        return res;
    }
    private void helper(List<Integer> res, TreeNode node) {
        if (node == null) {
            return;
        }
        res.add(node.val);
        helper(res, node.left);
        helper(res, node.right);
    }
}
```
### 非递归解法
```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Stack<TreeNode> s = new Stack<TreeNode>();
        if (root == null) {
            return res;
        }
        TreeNode node = root;
        while (!s.isEmpty() || node != null) {
            while (node != null) {
                s.push(node);
                res.add(node.val);
                node = node.left;
            }
            node = s.pop();
            node = node.right;

        }
        return res;
    }
}
```

### BFS
适用于自顶向下层序， 自底向上层序和锯齿层序遍历。
```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) {
            return res;
        }
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while (!q.isEmpty()) {
            int size = q.size();
            List<Integer> level = new ArrayList<>();
            for (int i =0; i < size; i++) {
                TreeNode node = q.poll();
                level.add(node.val);
                if (node.left != null) {
                    q.offer(node.left);
                }
                if (node.right != null) {
                    q.offer(node.right);
                }
            }
            res.add(level);
        }
        return res;

    }
}
```
