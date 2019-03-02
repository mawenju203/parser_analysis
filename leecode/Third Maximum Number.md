# 第三大的数
## 题目描述
给定一个非空数组，返回此数组中第三大的数。如果不存在，则返回数组中最大的数。要求算法时间复杂度必须是O(n)。

### 示例 1:
<li>输入: [3, 2, 1]</li>
<li>输出: 1</li>
<li>解释: 第三大的数是 1.</li>

### 示例 2:
<li>输入: [1, 2]</li>
<li>输出: 2</li>
<li>解释: 第三大的数不存在, 所以返回最大的数 2 .</li>

### 示例 3:
<li>输入: [2, 2, 3, 1]</li>
<li>输出: 1</li>
<li>解释: 注意，要求返回第三大的数，是指第三大且唯一出现的数。
存在两个值为2的数，它们都排第二。</li>

##
**网址：https://leetcode.com/problems/third-maximum-number/**


## 解题思路

使用map<int , int>; map ： key(nums中的数据) - value(数字出现的次数) ; map自动按照key排序;
逆迭代器遍历，找到对应的 k = 3 > map.size() ? 1 : 3 ; k即是逆迭代器的第k个数据 也是要求的结果；

**代码**

```
class Solution {
public:
    int thirdMax(vector<int>& nums) {
        map<int , int> con ;
        int c_i = 0 ;
        while (c_i < nums.size())
        {
            con[nums[c_i]] ++ ;
            c_i ++ ;
        }
        int k = 3 > con.size() ? 1 : 3 ;
        c_i = 0 ;
        for (auto rit = con.crbegin(); rit != con.crend(); ++rit)
        {
            c_i ++ ;
            if (k == c_i)
            {
                
                return  rit->first ;
            }
                
        }
        return -1 ;
    }
};
```
