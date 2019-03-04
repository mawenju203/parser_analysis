## 题目：最少移动次数使数组元素相等 II（462）

### 题目描述：
**给定一个非空整数数组，找到使所有数组元素相等所需的最小移动数，其中每次移动可将选定的一个元素加1或减1。 您可以假设数组的长度最多为10000。**

**例如:**

**输入:**
[1,2,3]

**输出:**
2

**说明：**
只有两个动作是必要的（记得每一步仅可使其中一个元素加1或减1）： 
[1,2,3]  =>  [2,2,3]  =>  [2,2,2]

#### 网址：https://leetcode.com/problems/minimum-moves-to-equal-array-elements-ii/

#### **解题思路:**

找到中位数：nth_element() ;可以在O(n)的时间内找到对应i的数据;无返回值;具体查看函数的定义;

**时间复杂度O(n);**

```
#include<algorithm>
class Solution {
public:

    int minMoves2(vector<int>& nums) {
        int res = 0, n = nums.size(), mid = n / 2;
        nth_element(nums.begin(), nums.begin() + mid, nums.end());
        for (int i = 0; i < n; ++i) {
            res += abs(nums[i] - nums[mid]);
        }
        return res;

    }
};
```
