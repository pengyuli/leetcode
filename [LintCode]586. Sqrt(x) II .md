# Sqrt(x) II

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
用二分法, 但是要注意0.01开根号是大于本身的, 所以取right的时候要在x和1中取最大值.
另外注意边界条件是left + 误差 < right
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
