# [LintCode]Sqrt(x) II

> Implement double sqrt(double x) and x >= 0.
> 
> Compute and return the square root of x.

	Example
	Example 1:
	
	Input: n = 2 
	Output: 1.41421356
	Example 2:
	
	Input: n = 3
	Output: 1.73205081

## 思路
与1不同的是这里是double, 所以会涉及到<1的情况. 这样一来, n可能是小于最后的结果的. 所以二分法的右边界不能单纯的设为n, 而要设为Math.min(n, 1)
### 代码
```
public class Solution {

    public double sqrt(double x) {
        double left = 0;
        double right = Math.max(1, x);
        while (left + 1e-10 < right) {
            double mid = left + (right - left) /2;
            if (mid * mid < x) {
                left = mid;
            } else {
                right = mid;
            }
        }
        return left;
    }
}
```
