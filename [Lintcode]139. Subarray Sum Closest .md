# Subarray Sum Closest

> Description

> Given an integer array, find a subarray with sum closest to zero. Return the indexes of the first number and last number.

> Example
> Example1
> 
> Input: 
> [-3,1,1,-3,5] 
> Output: 
> [0,2]
> Explanation: [0,2], [1,3], [1,1], [2,2], [0,4]

## 思路
遇到子列表sum类的题目都要先得到一个prefixSum列表, prefix_sum[i] = nums[0] + nums[1] + .. + nums[i - 1] and prefix_sum[0] = 0. 这道题其实就是找prefixSum列表中最接近的两个值, 如果可以sort一下列表, 就能用nlogn的复杂度解决. 但是sort之后就会失去了列表的index, 所以我们可以创建一个datastructure, 使得prefixSum[i]既包含前i-1个数的和, 又包含index.

这道题还有一个需要考虑的情况就是因为prefixSum[i] = nums[0] + nums[1] + .. + nums[i - 1], 当prefixSum[j]-prefixSum[i]的值最小的话, 那么sum的是nums[i+1-1] --> nums[j-1] 所以index是i->j-1 而不是i-1 -> j-1

### 复杂度
time O n(logn) space O(n)
### 代码
```
public class Solution {
    class pair {
        int val;
        int index;
        pair (int val, int index) {
            this.val = val;
            this.index = index;
        }
    }
    public int[] subarraySumClosest(int[] nums) {
        int res[] = new int[2];
        if (nums == null || nums.length <= 1) {
            return res;
        }
        pair[] sum = new pair[nums.length +1];
        sum[0] = new pair(0,-1);
        for (int i = 1; i < sum.length; i++) {
            sum[i] = new pair(nums[i-1] +sum[i-1].val, i-1);
        }
        Arrays.sort(sum, (a, b) -> a.val - b.val);
        
        int min = Integer.MAX_VALUE;
        for (int i = 1; i < sum.length; i++) {
            if (sum[i].val - sum[i-1].val <min) {
                min = sum[i].val - sum[i-1].val;
                if (sum[i].index <sum[i-1].index) {
                    res[0] = sum[i].index+1;
                    res[1] = sum[i-1].index;
                } else {
                    res[0] = sum[i-1].index+1;
                    res[1] = sum[i].index;
                }
            }
        }
        return res;
    }
}


```

