# Backtracking
这一类题通常都用递归的DFS方法, 来求取所有可能性.

### 递归三要素
一般来说,如果面试官不特别要求的话,DFS都可以使用递归(Recursion)的方式来实现。
这类题通常先把给的数组排序, 然后根据递归的方定义来写函数

递归三要素是实现递归的重要步骤:

- 递归的定义

- 递归的拆解

	- 递归的时候传入什么样的参数 (0, pos or pos + 1)
  		1. （对于在dfs内recursive的使用dfs时候 ） 如果每次是从i开始扫， 则得到的结果是不会重复的。（会出现[1,2]不会出现[2,1]）

  		2.  但是如果[1,2] 和[2,1]两个重复情况都要的话, 则要每次从0开始扫.并且要加一个boolean[] visit 来记录某些点是否被访问过

	- **去重**

	  如果已选取[1,2(1)] 那么[1,2(2)]就是重复选取 为了确保这种情况不发生，
	  只要确定当前所选取的值num[i]和num[i-1]不重复就可以， 如果重复跳过num[i].
	  对于每次从i扫的情况 ：
	  ```
	  if ( i != pos && num[i] == num[i - 1]) {
	                  continue;
	              }    
	  ```            
	  对于每次从0开始扫的情况：
	  ```
	  if (i > 0 && num[i] == num[i - 1] && !visit[i - 1]){
	                  continue;
	                  // important: if previous number have same value as (i)th
	                  // but have never been visited, then skip current number
	              }
	  ```

- 递归的出口 (递归返回条件, 需要判定每得到一个结果就返回还是特定条件才返回)
