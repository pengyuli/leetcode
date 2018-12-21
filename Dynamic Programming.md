# Dynamic Programming

## DP适用的类型
- 求可行性
- 求最值
- 求总方案数 (如果求具体方案则应该用dfs)

## DP使用条件
1.  最优化原理：如果问题的最优解所包含的子问题的解也是最优的，就称该问题具有最优子结构，即满足最优化原理。
2. 无后效性：即某阶段状态一旦确定，就不受这个状态以后决策的影响。也就是说，某状态以后的过程不会影响以前的状态，只与当前状态有关。
3. 有重叠子问题：即子问题之间是不独立的，一个子问题在下一阶段决策中可能被多次使用到。*（该性质并不是动态规划适用的必要条件，但是如果没有这条性质，动态规划算法同其他算法相比就不具备优势）*

## DP解法
1.  状态 State

2. 方程 Function
状态之间的联系，怎么通过小的状态，来算大的状态

3. 初始化 Intialization
最极限的小状态是什么, 起点

4. 答案 Answer
最大的那个状态是什么，终点

#### Matrix DP

- state: f[x][y] 表示我从起点走到 坐 标x,y……
- function: 研究走到xy 这个点之前的一步是从哪里走的
- intialize: 起点
- answer: 终点

#### Sequence Dp
- state: f[i]表示“ 前i”个位置/数字/字母,(以第i个为)...
- function: f[i] = f[j] … j 是i之前的一个位置
- intialize: f[0]..
- answer: f[n-1]..


#### Two Sequences Dp

- state: f[i][j]代表了第一个sequence的前i个数字/字符 配上第二个sequence的前j个...
- function: f[i][j] = 研究第i个和第j个的匹配关系
- intialize: f[i][0] 和 f[0][i](二维数组都要初始化第0行和第0列)
- answer: f[s1.length()][s2.length()]