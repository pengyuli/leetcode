# Copy Books.md

> Given n books and the i-th book has pages[i] pages. There are k persons to copy these books.
> 
> These books list in a row and each person can claim a continous range of books. For example, one copier can copy the books from i-th to j-th continously, but he can not copy the 1st book, 2nd book and 4th book (without 3rd book).
> 
> They start copying books at the same time and they all cost 1 minute to copy 1 page of a book. What's the best strategy to assign books so that the slowest copier can finish at earliest time?
> 
> Return the shortest time that the slowest copier spends.


	Example
	Example 1:
	
	Input: pages = [3, 2, 4], k = 2
	Output: 5
	Explanation: 
	    First person spends 5 minutes to copy book 1 and book 2.
	    Second person spends 4 minutes to copy book 3.
	Example 2:
	
	Input: pages = [3, 2, 4], k = 3
	Output: 4
	Explanation: Each person copies one of the books.

## 思路
存在一个ans, 使得当每个人拿页数上线是ans时候需要的人数<= k. 所以可以二分查找这个ans, 如果需要的人数>k 说明每个人拿的页码不够需要ans更大

### 复杂度
O(n * long(MAX_VALUE))
### 代码
```
public class Solution {
    /**
     * @param pages: an array of integers
     * @param k: An integer
     * @return: an integer
     */
    public int copyBooks(int[] pages, int k) {
       if (pages == null || pages.length == 0) {
           return 0;
       }
       int left = 0, right = Integer.MAX_VALUE;
       while (left + 1 < right) {
           int mid = (right + left) /2;
           if (checkAnswer(pages, k, mid)) {
               right = mid;
           } else {
               left = mid;
           }
       }
       if (checkAnswer(pages, k, left)) return left;
       return right;
    }
    
    private boolean checkAnswer(int[] pages, int k, int ans) {
        int count = 0;
        int sum = 0;
        for (int i = 0; i < pages.length; i++) {
            if (pages[i] > ans) {
                return false;
            }
            if (sum+ pages[i] > ans) {
                count++;
                sum = 0;
            }
            sum+= pages[i];
        }
        if (sum > 0) count++;
        return count <= k;
    }
}

```