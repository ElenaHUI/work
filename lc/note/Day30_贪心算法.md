# Day30_贪心算法03
## 452. 用最少数量的箭引爆气球

[力扣题目链接](https://leetcode.cn/problems/minimum-number-of-arrows-to-burst-balloons/)

### 题目

有一些球形气球贴在一堵用 XY 平面表示的墙面上。墙面上的气球记录在整数数组 `points` ，其中`points[i] = [xstart, xend]` 表示水平直径在 `xstart` 和 `xend`之间的气球。你不知道气球的确切 y 坐标。

一支弓箭可以沿着 x 轴从不同点 **完全垂直** 地射出。在坐标 `x` 处射出一支箭，若有一个气球的直径的开始和结束坐标为 `x``start`，`x``end`， 且满足  `xstart ≤ x ≤ x``end`，则该气球会被 **引爆** 。可以射出的弓箭的数量 **没有限制** 。 弓箭一旦被射出之后，可以无限地前进。

给你一个数组 `points` ，*返回引爆所有气球所必须射出的 **最小** 弓箭数* 。

 

**示例 1：**

```
输入：points = [[10,16],[2,8],[1,6],[7,12]]
输出：2
解释：气球可以用2支箭来爆破:
-在x = 6处射出箭，击破气球[2,8]和[1,6]。
-在x = 11处发射箭，击破气球[10,16]和[7,12]。
```

**示例 2：**

```
输入：points = [[1,2],[3,4],[5,6],[7,8]]
输出：4
解释：每个气球需要射出一支箭，总共需要4支箭。
```

**示例 3：**

```
输入：points = [[1,2],[2,3],[3,4],[4,5]]
输出：2
解释：气球可以用2支箭来爆破:
- 在x = 2处发射箭，击破气球[1,2]和[2,3]。
- 在x = 4处射出箭，击破气球[3,4]和[4,5]。
```

 



**提示:**

- `1 <= points.length <= 105`
- `points[i].length == 2`
- `-231 <= xstart < xend <= 231 - 1`

### 题解

一道计算重叠区间的题

- ==sort==函数的妙用

在 C++ 中，`std::sort` 函数的第三个参数是一个可选的比较函数（或函数对象、lambda 表达式），用于定义元素之间的排序规则。这个参数决定了元素如何被比较和排列的方式。

**比较函数的作用**

比较函数是一个二元谓词，它接受两个参数（待比较的元素），并返回一个布尔值，表示这两个元素的顺序关系。具体来说：

- 如果返回 `true`，表示第一个参数应该排在第二个参数之前。
- 如果返回 `false`，表示顺序可以相反。

```cpp
template< class RandomIt, class Compare >
void sort( RandomIt first, RandomIt last, Compare comp );
```

- `first`, `last`：定义排序范围的迭代器。
- `comp`：可选的比较函数，必须是一个**严格弱序**（Strict Weak Ordering）的比较函数。

**严格弱序的要求**

比较函数 `comp(a, b)` 必须满足以下性质：

1. **非自反性**：`comp(a, a)` 必须为 `false`。
2. **非对称性**：如果 `comp(a, b)` 为 `true`，则 `comp(b, a)` 必须为 `false`。
3. **传递性**：如果 `comp(a, b)` 和 `comp(b, c)` 都为 `true`，则 `comp(a, c)` 也必须为 `true`。
4. **传递性等价**：如果 `a` 和 `b` 是等价的（即 `!comp(a, b)` 且 `!comp(b, a)`），并且 `b` 和 `c` 也是等价的，则 `a` 和 `c` 也必须是等价的。
**示例分析**
在你的代码中，比较函数 `cmp` 的定义如下：
```cpp
static bool cmp(const vector<int>& a, const vector<int>& b) {
    return a[0] < b[0];
}
```
这个比较函数告诉 `sort`：

- **排序键**：使用每个区间的第一个元素（左端点）作为排序依据。
- **顺序**：左端点较小的区间排在前面。

- `LLONG_MIN`是`long long`的最小值

```c++
class Solution {
public:
    static bool cmp(vector<int>& a,vector<int>& b){
        return a[1]<b[1];
    }
    int findMinArrowShots(vector<vector<int>>& points) {
        sort(points.begin(),points.end(),cmp);
        long long pre = LLONG_MIN;
        int ans = 0;
        for(auto& p : points) {
            if(p[0]>pre){
                ans++;
                pre = p[1];
            }
        }
        return ans;
    }
};
```

## 435. 无重叠区间

### 题目

给定一个区间的集合 `intervals` ，其中 `intervals[i] = [starti, endi]` 。返回 *需要移除区间的最小数量，使剩余区间互不重叠* 。

**注意** 只在一点上接触的区间是 **不重叠的**。例如 `[1, 2]` 和 `[2, 3]` 是不重叠的。

 

**示例 1:**

```
输入: intervals = [[1,2],[2,3],[3,4],[1,3]]
输出: 1
解释: 移除 [1,3] 后，剩下的区间没有重叠。
```

**示例 2:**

```
输入: intervals = [ [1,2], [1,2], [1,2] ]
输出: 2
解释: 你需要移除两个 [1,2] 来使剩下的区间没有重叠。
```

**示例 3:**

```
输入: intervals = [ [1,2], [2,3] ]
输出: 0
解释: 你不需要移除任何区间，因为它们已经是无重叠的了。
```

 

**提示:**

- `1 <= intervals.length <= 105`
- `intervals[i].length == 2`
- `-5 * 104 <= starti < endi <= 5 * 104`

### 题解

和之前那道题也是一模一样啊，只不过这题问要使得剩余区间不相互重叠，我们可以找到==最多的不相互重叠的区间==数量，然后用==总数==减一下

1. 按右端点排序。这里可以试试举反例，有几种方法，左端点，区间长度，发现都不行，只有右端点可以
2. 按右端点最早结束开始遍历如果左端点大于等于之前的右端点，说明可以把这一段加进最多不重复集合，`max++`
3. 最后用总数减去最多不重叠区间的数量，就是最少要删的数量

```c++
class Solution {
public:
    static bool cmp(vector<int>& a,vector<int>& b){
        return a[1]<b[1];
    }
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        sort(intervals.begin(),intervals.end(),cmp);
        long long pre = LLONG_MIN;
        int max = 0;
        for(auto& i : intervals){
            if(i[0]>=pre){
                max++;
                pre = i[1];
            }
        }
        return intervals.size() - max;
    }
};
```

## 63.划分字母区间

### 题目

给你一个字符串 `s` 。我们要把这个字符串划分为尽可能多的片段，同一字母最多出现在一个片段中。例如，字符串 `"ababcc"` 能够被分为 `["abab", "cc"]`，但类似 `["aba", "bcc"]` 或 `["ab", "ab", "cc"]` 的划分是非法的。

注意，划分结果需要满足：将所有划分结果按顺序连接，得到的字符串仍然是 `s` 。

返回一个表示每个字符串片段的长度的列表。

 

**示例 1：**

```
输入：s = "ababcbacadefegdehijhklij"
输出：[9,7,8]
解释：
划分结果为 "ababcbaca"、"defegde"、"hijhklij" 。
每个字母最多出现在一个片段中。
像 "ababcbacadefegde", "hijhklij" 这样的划分是错误的，因为划分的片段数较少。 
```

**示例 2：**

```
输入：s = "eccbbbbdec"
输出：[10]
```

 

**提示：**

- `1 <= s.length <= 500`
- `s` 仅由小写英文字母组成

### 题解

==同一字母最多出现在一个片段中==意味着，一个片段若要包含字母 a，那么所有的字母 a 都必须在这个片段中。

例如，`s=ababcbacadefegdehijhklij`，其中字母 a 的下标在区间` [0,8]` 中，那么包含 a 的片段至少要包含区间` [0,8]`。

把所有出现在 s 中的字母及其下标区间列出来：

字母	下标	下标区间
a	[0,2,6,8]	[0,8]
b	[1,3,5]	[1,5]
c	[4,7]	[4,7]
d	[9,14]	[9,14]
e	[10,12,15]	[10,15]
f	[11]	[11,11]
g	[13]	[13,13]
h	[16,19]	[16,19]
i	[17,22]	[17,22]
j	[18,23]	[18,23]
k	[20]	[20,20]
l	[21]	[21,21]
例如字母 d 的区间为 [9,14]，片段要包含 d，必须包含区间 [9,14]，但区间 [9,14] 中还有其它字母 e,f,g，所以该片段也必须包含这些字母对应的区间 [10,15],[11,11],[13,13]，合并后得到区间 [9,15]。

将表格中的区间合并为如下几个大区间：

所以该问题转化为==区间合并==问题

[0,8],[9,15],[16,23]
这些区间满足==同一字母最多出现在一个片段中==的要求。

由于题目要求划分出尽量多的片段，而我们又无法将上述区间的任何区间划分开，所以合并出的区间长度即为答案。

1. 遍历 s，计算字母 c 在 s 中的最后出现的下标 last[c]。
2. 初始化当前正在合并的区间左右端点 start=0, end=0。
3. 再次遍历 s，由于当前区间必须包含所有 s[i]，所以用 last[s[i]] 更新区间右端点 end 的最大值。
4. 如果发现 end=i，那么当前区间合并完毕，把区间长度 end−start+1 加入答案。然后更新 start=i+1 作为下一个区间的左端点。
5. 遍历完毕，返回答案。

```c++
class Solution {
public:
    vector<int> partitionLabels(string s) {
        int n = s.length();
        int last[26];//记录每个字母最后出现的位置
        for(int i=0;i<n;i++){
            last[s[i]-'a']=i;//记录每个字母最后出现的位置
        }
        vector<int> ans;
        int start = 0, end = 0;//初始化
        for(int i = 0;i<n;i++){
            end = max(end,last[s[i]-'a']);
            if(end == i){//如果当前点就是区间末端说明这个区间结束了
                ans.push_back(end-start+1);
                start = i+1;//下一个区间的左端点
            }
        }
        return ans;
    }
};
```



