# Day25_回溯算法

## 491.递增子序列

[力扣题目链接](https://leetcode.cn/problems/non-decreasing-subsequences/)

### 题目

给你一个整数数组 `nums` ，找出并返回所有该数组中不同的递增子序列，递增子序列中 **至少有两个元素** 。你可以按 **任意顺序** 返回答案。

数组中可能含有重复元素，如出现两个整数相等，也可以视作递增序列的一种特殊情况。

 

**示例 1：**

```
输入：nums = [4,6,7,7]
输出：[[4,6],[4,6,7],[4,6,7,7],[4,7],[4,7,7],[6,7],[6,7,7],[7,7]]
```

**示例 2：**

```
输入：nums = [4,4,3,2,1]
输出：[[4,4]]
```

 

**提示：**

- `1 <= nums.length <= 15`
- `-100 <= nums[i] <= 100`

### 题解

- 关键是利用`unordereed_set`也就是哈希表，来记录字符有没有出现过来==去重==

其他的就是回溯算法三部曲，终止条件，层内循环，以及参数传递，这里有一个很妙的地方，每一次递归都会创建一个新的`set`也就是不用对其清空，下一层会自动开一个新的

```c++
class Solution {
public:
    vector<vector<int>> ans;
    vector<int> path;
    void Find(vector<int>& nums,int idx,int pre){
        int n = nums.size();
        if(idx == n)return;
        unordered_set<int> used;
        for(int i = idx;i < n;i++){
            if(nums[i] >= pre && used.find(nums[i])==used.end()){
                path.push_back(nums[i]);
                used.insert(nums[i]);
                if(path.size()>=2)ans.push_back(path);
                Find(nums,i+1,nums[i]);
                path.pop_back();
            }
        }
    }
    vector<vector<int>> findSubsequences(vector<int>& nums) {
        Find(nums,0,-101);
        return ans;
    }
};
```

- 此外这里由于数据最小就是`-100`所以默认值设为`-101`实际上只要判断一下如果`path`里面是空的就直接加入就好了

## 46.全排列

### 题目

给定一个不含重复数字的数组 `nums` ，返回其 *所有可能的全排列* 。你可以 **按任意顺序** 返回答案。

 

**示例 1：**

```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**示例 2：**

```
输入：nums = [0,1]
输出：[[0,1],[1,0]]
```

**示例 3：**

```
输入：nums = [1]
输出：[[1]]
```

 

**提示：**

- `1 <= nums.length <= 6`
- `-10 <= nums[i] <= 10`
- `nums` 中的所有整数 **互不相同**

### 题解

```c++
class Solution {
public:
    vector<int> path;
    vector<vector<int>> ans;
    void Find(vector<int>& nums,vector<bool>& used){
        if(path.size()==nums.size()){
            ans.push_back(path);
            return;
        }
        for(int i = 0;i < nums.size();i++){
            if(used[i]==false){
                used[i] = true;
                path.push_back(nums[i]);
                Find(nums,used);
                path.pop_back();
                used[i] = false;
            }
            
        }
    }
    vector<vector<int>> permute(vector<int>& nums) {
        int n = nums.size();
        vector<bool> used(n+1,false);
        Find(nums,used);
        return ans;
    }
};
```

- 用`used`数组来存访问情况，已经访问过的就跳过，并且由于不是在层内，是随着一层层深入，所以不能每层新开一个，而是要当参数一直传递，并且不访问了之后还要剔除掉
- 在主函数加入`vector<bool> used(n+1,false);`并且将这个数组随着参数传递即可

## 47.全排列 II

[力扣题目链接](https://leetcode.cn/problems/permutations-ii/)

### 题目

给定一个可包含重复数字的序列 `nums` ，***按任意顺序*** 返回所有不重复的全排列。

 

**示例 1：**

```
输入：nums = [1,1,2]
输出：
[[1,1,2],
 [1,2,1],
 [2,1,1]]
```

**示例 2：**

```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

 

**提示：**

- `1 <= nums.length <= 8`
- `-10 <= nums[i] <= 10`

### 题解

- 去重喵

```c++
class Solution {
public:
    vector<vector<int>> ans;
    vector<int> path;
    void Find(vector<int>& nums,vector<bool> used){
        if(path.size()==nums.size()){
            ans.push_back(path);
            return;
        }
        for(int i = 0;i<nums.size();i++){
            if (i > 0 && nums[i] == nums[i - 1] && used[i - 1] == false) {
                continue;
            }
            if(used[i]==false){
                used[i]=true;
                path.push_back(nums[i]);
                Find(nums,used);
                path.pop_back();
                used[i] = false;
            }
        }
    }
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        vector<bool> used(nums.size()+1,false);
        Find(nums,used);
        return ans;
    }
};
```

- 重复元素的去重，和之前的一样，先排序，和前一个一样且前一个没有使用过的话就说明选了这个和选了前一个一样，也就是说不需要了`if (i > 0 && nums[i] == nums[i - 1] && used[i - 1] == false) {continue;}`

