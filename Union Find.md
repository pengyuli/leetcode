# Union Find
## 复杂度
Find: O(ɑ(n))* ≈ O(1)

Union: O(ɑ(n))* ≈ O(1)

Space: O(n)
## 模板
```
class UF{
    int[] parent;
    int[] rank;
    
    public UF(int size) {
        parent = new int[size];
        rank = new int[size];
        for (int i = 0; i < size; i++) {
            parent[i] = i;
            rank[i] = 1;
        }
    }
    public int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]);
        }
        return parent[x];
    }
    public void union(int x, int y) {
        int xr = find(x);
        int yr = find(y);
        if (xr == yr) {
            
        } else if (rank[xr] < rank[yr]) {
            parent[xr] = yr;
        } else if (rank[xr] > rank[yr]) {
            parent[yr] = xr;
        } else {
            parent[xr] = yr;
            rank[yr]++;
        }
    }
}
```
