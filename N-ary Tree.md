# N-ary Tree
## n叉树的遍历

### preOrder
#### 递归
```java
class Solution {
    public List<Integer> preorder(Node root) {
        List<Integer> res = new ArrayList<>();
        helper(root, res);
        return res;
    }
    private void helper(Node node, List<Integer> res) {
        if (node != null) {
            res.add(node.val);
            for (Node i : node.children) {
            helper(i, res);
            }
        }
        
    }
}
```

#### 迭代
```
class Solution {
    public List<Integer> preorder(Node root) {
        List<Integer> res = new ArrayList<>();
        if (root == null) {
            return res;
        }
        Stack<Node> stack = new Stack<>();
        stack.push(root);
        while (!stack.isEmpty()) {
            Node node = stack.pop();
            res.add(node.val);
            for (int i = node.children.size() - 1; i>= 0; i--) {
                stack.push(node.children.get(i));
            }
        }
        return res;
    }
}
```

## 递归
### top-down
```
1. return specific value for null node
2. update the answer if needed                              // answer <-- params
3. for each child node root.children[k]:
4.      ans[k] = top_down(root.children[k], new_params[k])  // new_params <-- root.val, params
5. return the answer if needed                              // answer <-- all ans[k]
```

### bottom-up
```
1. return specific value for null node
2. for each child node root.children[k]:
3.      ans[k] = bottom_up(root.children[k])    // call function recursively for all children
4. return answer                                // answer <- root.val, all ans[k]

```