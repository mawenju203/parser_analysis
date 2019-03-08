@[TOC](题目：390. 消除游戏)
## 题目描述
**网址：https://leetcode-cn.com/problems/elimination-game/**  
**博客地址：https://mp.csdn.net/mdeditor/88342466#**  

给定一个从1 到 n 排序的整数列表。  
首先，从左到右，从第一个数字开始，每隔一个数字进行删除，直到列表的末尾。  
第二步，在剩下的数字中，从右到左，从倒数第一个数字开始，每隔一个数字进行删除，直到列表开头。  
我们不断重复这两步，从左到右和从右到左交替进行，直到只剩下一个数字。  
返回长度为 n 的列表中，最后剩下的数字。  

**示例：**

```model
输入:
    n = 9,
    1 2 3 4 5 6 7 8 9
    2 4 6 8
    2 6
    6
输出:
	6
```


#### 解题思路：  
找每一轮的起始位置和终止位置；终止位置作为下一轮的起始位置；  
每轮数据剩余 n >> 1 ；数据之间间隔变为原先的2倍；  

**第一张是原理图；第二张为 n = 23的计算过程（原理实现）；**  
<center>
<img id="replation" src="https://img-blog.csdnimg.cn/20190308102357883.jpg"  width="90%" height="80%">
<img id="replation" src="https://img-blog.csdnimg.cn/20190308102243315.jpg"  width="90%" height="30%">
</center>

**代码：**
```answer
class Solution {
public:
    int lastRemaining(int n) {
        int startN = 1 ;
        bool station = true ;
        int num2 = 1 ;
        while (n != 1)
        {
            if (n & 1)
                n -- ;
            if (station)
            {
                startN += (n - 1) * num2 ;
                station = false ;
            }
            else
            {
                startN -= (n - 1) * num2 ;
                station = true ;
            }
            n >>= 1 ;
            num2 <<= 1 ;
        }
        return startN ;
    }
};
```
