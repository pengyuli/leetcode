#  用递归的解法来求解树的问题

## 自顶向下递归 top-down

### 思路
Top-down 的解法是在每一步的递归过程中, 先根据当前节点得到一个结果, 然后再递归的访问他的子节点并且将当前得到的结果传到下一个递归过程中, 简单的的说就是**先处理再递归**, 访问方式类似preorder. **DFS**

1. return specific value for null node
2. update the answer if needed                      // anwer <-- params
3. left_ans = top_down(root.left, left_params)      // left_params <-- root.val, params
4. right_ans = top_down(root.right, right_params)   // right_params <-- root.val, params
5. return the answer if needed                      // answer <-- left_ans, right_ans

#### code示范
```
    public int maxDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        helper(root, 1);
        return ans;
    }
    private void helper(TreeNode node, int depth) {
        if (node == null) {
            return;
        }
        if (node.left == null && node.right == null) {
            ans = Math.max(depth, ans);
        }
        helper(node.left, depth + 1);
        helper(node.right, depth + 1);
    }
```

## 自底向上递归 bottom-up
bottom-up 的解法就是先层层递归得到最根节点的结果, 然后再将底层的结果返回到上一层处理, 直到root. 简单地说就是**先递归再处理**, 访问方式类似postorder.
### 思路
```
1. return specific value for null node
2. left_ans = bottom_up(root.left)          // call function recursively for left child
3. right_ans = bottom_up(root.right)        // call function recursively for right child
4. return answers                           // answer <-- left_ans, right_ans, root.val
```
### 代码示范
```
public int maximum_depth(TreeNode root) {
	if (root == null) {
		return 0;                                   // return 0 for null node
	}
	int left_depth = maximum_depth(root.left);
	int right_depth = maximum_depth(root.right);
	return Math.max(left_depth, right_depth) + 1;	// return depth of the subtree rooted at root
}
```
## 心得
tree的题目通常情况系下需要维护一个全局变量type作为outpu, 递归的子函数通常返回int(高度相关) boolean(可行性相关) 