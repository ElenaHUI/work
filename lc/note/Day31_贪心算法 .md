# Day31_贪心算法
## 56. 合并区间

[力扣题目链接](https://leetcode.cn/problems/merge-intervals/)

### 题目

以数组 `intervals` 表示若干个区间的集合，其中单个区间为 `intervals[i] = [starti, endi]` 。请你合并所有重叠的区间，并返回 *一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间* 。

 

**示例 1：**

```
输入：intervals = [[1,3],[2,6],[8,10],[15,18]]
输出：[[1,6],[8,10],[15,18]]
解释：区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
```

**示例 2：**

```
输入：intervals = [[1,4],[4,5]]
输出：[[1,5]]
解释：区间 [1,4] 和 [4,5] 可被视为重叠区间。
```

 

**提示：**

- `1 <= intervals.length <= 104`
- `intervals[i].length == 2`
- `0 <= starti <= endi <= 104`

### 题解

老样子，先排序，再合并

```c++
class Solution {
public:
    static bool cmp(vector<int>& a,vector<int>& b){
        return a[0]<b[0];
    }
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(),intervals.end(),cmp);
        int n = intervals.size();
        int start = intervals[0][0];
        int end = intervals[0][1];
        vector<vector<int>> ans;
        for(int i=0;i<n;i++){
            if(intervals[i][0] <= end){
                end = max(intervals[i][1],end);
            }else{
                ans.push_back({start,end});
                start = intervals[i][0];
                end = intervals[i][1];
            }
        }
        ans.push_back({start,end});
        return ans;
    }
};
```

## 738.单调递增的数字

[力扣题目链接](https://leetcode.cn/problems/monotone-increasing-digits/)

### 题目

当且仅当每个相邻位数上的数字 `x` 和 `y` 满足 `x <= y` 时，我们称这个整数是**单调递增**的。

给定一个整数 `n` ，返回 *小于或等于 `n` 的最大数字，且数字呈 **单调递增*** 。

 

**示例 1:**

```
输入: n = 10
输出: 9
```

**示例 2:**

```
输入: n = 1234
输出: 1234
```

**示例 3:**

```
输入: n = 332
输出: 299
```

 

**提示:**

- `0 <= n <= 109`

### 题解

我的大概思路就是最高字母尽量取大，不行就少一位全是9

这是一道很明显的贪心题目。既然要尽可能的大，那么这个数从高位开始要尽可能地保持不变。那么我们找到从高到低第一个满足`str[i] > str[i+1]`的位置，因为如果`str[i] <= str[i+1]`的话，就还符合题目要求着呢，这时候把这一位减一然后后面变成9（因为要小于当前数据），显然不比不变来的大。然后把 `str[i]−1 `，再把后面的位置都变成 9 即可。对应可看下面的例子。

```c++
n   = 1234321
res = 1233999
```

但是由于`str[i]-1`以后，不满足`str[i−1]<=str[i]`了，所以我们在分析下这种情况怎么处理。我们看下这种情况的例子：

```c++
n    = 2333332
res  = 2299999
```

么肯定有` str[i−1]==str[i]`，此时就需要再` str[i−1]−1`，递归地会处理到某个位置 `idx`，我们发现 `str[idx]==str[idx+1]==...=str[i]` 。然后只要`str[idx]−1`，然后后面都补上 9 即可。

所以目的就是找到一个变成9的位置，该位置前面都递增

```c++
class Solution {
public:
    int monotoneIncreasingDigits(int n) {
        string NumStr = to_string(n);
        int len = NumStr.size();
        int flag = len;//从最后一位开始变9
        for(int i = len-1 ;i > 0;i--){
            if(NumStr[i-1]>NumStr[i]){//不符合条件
                NumStr[i-1]--;//前一位-1
                flag=i;//从i开始变9
            }            
        }
        for(int i = flag;i < len;i++){
            NumStr[i] = 9;
        }
        n = stoi(NumStr);
        return n;
    }
};
```

