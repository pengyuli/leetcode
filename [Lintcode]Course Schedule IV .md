# Course Schedule IV
> There are a total of n courses you have to take, labeled from 0 to n - 1.
> Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]
> 
> Given the total number of courses and a list of prerequisite pairs, return the number of different ways you can get all the lessons

	Example
	Example 1
	
	Input:
	n = 2
	prerequisites = [[1,0]]
	Output: 1
	Explantion:
	You must have class in order 0->1.
	Example 2
	
	Input:
	n = 2
	prerequisites = []
	Output: 2
	Explanation:
	You can have class in order 0->1 or 1->0.

## 思路 拓扑排序+dfs
```
public class Solution {
    /**
     * @param n: an integer, denote the number of courses
     * @param p: a list of prerequisite pairs
     * @return: return an integer,denote the number of topologicalsort
     */
    List<Integer>[] graph;
    int[] indegree;
    int result;
    boolean[] visit;
    int n;
    public int topologicalSortNumber(int n, int[][] p) {
        graph =new ArrayList[n];
        indegree = new int[n];
        visit = new boolean[n];
        result = 0;
        this.n = n;
        for (int i = 0; i < n; i++) {
            graph[i] = new ArrayList<>();
        }
        for (int i = 0; i < p.length; i++) {
            
            graph[p[i][1]].add(p[i][0]);
            indegree[p[i][0]]++;
        }
        dfs(0);
        return result;
       
    }
    
    private void dfs(int count) {
        if (count == n) {
            result++;
            return;
        }
        for (int i = 0; i < n; i++) {
            if (!visit[i] && indegree[i] == 0) {
                visit[i] = true;
                for (int j = 0; j < graph[i].size(); j++) {
                    indegree[graph[i].get(j)]--;
                }
                dfs(count+1);
                visit[i] = false;
                for (int j = 0; j < graph[i].size(); j++) {
                    indegree[graph[i].get(j)]++;
                }
            }
        }
    }
}
```