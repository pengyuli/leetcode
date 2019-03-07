# Array

## Two Pointer

## 滑动窗口

## 各个方向取值
```
int[][] dir = {{0,1}, {0,-1}, {-1,0}, {-1,-1}, {-1, 1}, {1,1}, {1,0}, {1,-1}};
for (int i = 0; i< row; i++) {
            for (int j = 0; j < column; j++) {
                for (int[] d: dir) {
                    if (i+d[0] < 0 || i +d[0] >= row || j + d[1] < 0 || j + d[1] >= column) {
                        continue;
                    }
                }
            }

```