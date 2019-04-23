# [Lintcode]Number of Airplanes in the Sky

> 给出飞机的起飞和降落时间的列表，用序列 interval 表示. 请计算出天上同时最多有多少架飞机？
> 
> 如果多架飞机降落和起飞在同一时刻，我们认为降落有优先权。
> 
> Have you met this question in a real interview?  
> Example
> 样例 1:
> 
> 输入: [(1, 10), (2, 3), (5, 8), (4, 7)]
> 输出: 3
> 解释: 
> 第一架飞机在1时刻起飞, 10时刻降落.
> 第二架飞机在2时刻起飞, 3时刻降落.
> 第三架飞机在5时刻起飞, 8时刻降落.
> 第四架飞机在4时刻起飞, 7时刻降落.
> 在5时刻到6时刻之间, 天空中有三架飞机.
> 样例 2:
> 
> 输入: [(1, 2), (2, 3), (3, 4)]
> 输出: 1
> 解释: 降落优先于起飞.

## 思路
用扫描线算法, 把所有开始时间和结束时间分别加入一个数组并排序. 遍历两个数组, 如果当前结束时间>开始时间, 说明在最近一班飞机下降之前会有新的飞机飞上去数量+1, 反之则说明飞机需要先下降, 所以数量-1
### 复杂度
time O(nlogn) Space O(n)
### 代码
```
public int countOfAirplanes(List<Interval> airplanes) {
        if (airplanes == null || airplanes.size() == 0) {
            return 0;
        }
        int count = 0;
        int size = airplanes.size();
        int[] s = new int[size];
        int[] e = new int[size];
        for (int i = 0; i < size;i++) {
            s[i] = airplanes.get(i).start;
            e[i] = airplanes.get(i).end;
        }
        Arrays.sort(s);
        Arrays.sort(e);
        int index = 0;
        for (int i = 0; i < s.length; i++) {
            if (s[i] < e[index]) {
                count++;
            } else {
                index++;
            }
        }
        return count;
    }
}
```
