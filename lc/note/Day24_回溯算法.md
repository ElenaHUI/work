# 回溯算法03

## 93.复原IP地址

[力扣题目链接](https://leetcode.cn/problems/restore-ip-addresses/)

==中等== ==40==

**有效 IP 地址** 正好由四个整数（每个整数位于 `0` 到 `255` 之间组成，且不能含有前导 `0`），整数之间用 `'.'` 分隔。

- 例如：`"0.1.2.201"` 和` "192.168.1.1"` 是 **有效** IP 地址，但是 `"0.011.255.245"`、`"192.168.1.312"` 和 `"192.168@1.1"` 是 **无效** IP 地址。

给定一个只包含数字的字符串 `s` ，用以表示一个 IP 地址，返回所有可能的**有效 IP 地址**，这些地址可以通过在 `s` 中插入 `'.'` 来形成。你 **不能** 重新排序或删除 `s` 中的任何数字。你可以按 **任何** 顺序返回答案。

 

**示例 1：**

```
输入：s = "25525511135"
输出：["255.255.11.135","255.255.111.35"]
```

**示例 2：**

```
输入：s = "0000"
输出：["0.0.0.0"]
```

**示例 3：**

```
输入：s = "101023"
输出：["1.0.10.23","1.0.102.3","10.1.0.23","10.10.2.3","101.0.2.3"]
```

 

**提示：**

- `1 <= s.length <= 20`
- `s` 仅由数字组成

### 题解

和前面那道题很像，都是划分问题，只是判断条件不同

```c++
class Solution {
public:
    string ip;
    vector<string> ans;
    bool isIp(string s){//这里判断疑似有点生硬了，但是看了题解也是都挺生硬的
        if(s.size()==1&&s[0]<='9'&&s[0]>='0')return true;
        if(s.size()==2&&s[0]<='9'&&s[0]>'0'&&s[1]<='9'&&s[1]>='0')return true;
        if(s[0]<='9'&&s[0]>'0'&&s[1]<='9'&&s[1]>='0'&&s[2]<='9'&&s[2]>='0'&&stoi(s)<=255)return true;
        return false;
    }
    void find(string s,int start,int k){//k表示现在有几段了，超过4段肯定不用看了
        if(k>4)return;
        if(k==4){
            if(start==s.size())
            {ip.erase(ip.size()-1,1);ans.push_back(ip);}//也是有点笨，可以看后面的解法
            return;
        }
        for(int i=start;i<start+3&&i<s.size();i++){//每串字符不多于三个数，且没超过总长度
            string sub=s.substr(start,i-start+1);
            //cout<<sub;
            //cout<<isIp(sub)<<endl;
            if(!isIp(sub))continue;//跳过
            int p=ip.size();//记录插入的位置
            ip.append(sub);
            ip.append(".");//这里其实可以用insert（）直接加.详情见后面解法
            find(s,i+1,k+1);
            ip.erase(p,i-start+2);//回溯
        }
    }
    vector<string> restoreIpAddresses(string s) {
        find(s,0,0);
        return ans;
    }
};
```

我的题解有点愚钝，看看别人的

```c++
class Solution {
public:
    vector<string> ans;
    // 检查字符串是否为有效的IP段
    bool isValidSegment(string s) {
        int len = s.length();
        if (len == 0 || len > 3) return false;
        if (len > 1 && s[0] == '0') return false; // 不允许前导零
        int num = 0;
        for (char c : s) {//循环判断是不是数字，并计算总和，和stoi作用一样
            if (!isdigit(c)) return false;
            num = num * 10 + (c - '0');
        }
        return num >= 0 && num <= 255;
    }
    
    // 回溯函数
    void backtrack(string& s, int start, int k, string current) {
        int n = s.length();
        if (k == 4) {
            if (start == n) {
                ans.push_back(current.substr(0, current.length() - 1)); // 加入并移除末尾的点
            }
            return;
        }
        
        for (int len = 1; len <= 3; len++) {
            if (start + len > n) break;//防止越界
            string segment = s.substr(start, len);
            if (isValidSegment(segment)) {
                backtrack(s, start + len, k + 1, current + segment + ".");//不用append，直接加就完了
            }
        }
    }
    
    vector<string> restoreIpAddresses(string s) {
        backtrack(s, 0, 0, "");
        return ans;
    }
};
```

## 78.子集

[力扣题目链接](https://leetcode.cn/problems/subsets/)

==中等== ==15==

给你一个整数数组 `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。

解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集。

 

**示例 1：**

```
输入：nums = [1,2,3]
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

**示例 2：**

```
输入：nums = [0]
输出：[[],[0]]
```

 

**提示：**

- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`
- `nums` 中的所有元素 **互不相同**

### 题解

感觉可以用暴力枚举hh，但是还是用回溯法做一下啊

```c++
class Solution {
public:
    vector<int> sub;
    vector<vector<int>> ans;
    void find(vector<int>& nums,int i)
    {
        if(i==nums.size()){
            ans.push_back(sub);
            return;
        }
        sub.push_back(nums[i]);//选当前这个元素
        find(nums,i+1);
        sub.pop_back();//不选当前这个元素
        find(nums,i+1);
        return;
    }
    vector<vector<int>> subsets(vector<int>& nums) {
        find(nums,0);
        return ans;
    }
};
```

btw其实也可以看成是组合问题记录路径的情况

```c++
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& nums, int startIndex) {
        result.push_back(path); // 收集子集，要放在终止添加的上面，否则会漏掉自己
        if (startIndex >= nums.size()) { // 终止条件可以不加
            return;
        }
        for (int i = startIndex; i < nums.size(); i++) {
            path.push_back(nums[i]);
            backtracking(nums, i + 1);
            path.pop_back();
        }
    }
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        result.clear();
        path.clear();
        backtracking(nums, 0);
        return result;
    }
};

```

## 90.子集II

[力扣题目链接](https://leetcode.cn/problems/subsets-ii/)

==中等==

给你一个整数数组 `nums` ，其中可能包含重复元素，请你返回该数组所有可能的 子集（幂集）。

解集 **不能** 包含重复的子集。返回的解集中，子集可以按 **任意顺序** 排列。

 

**示例 1：**

```
输入：nums = [1,2,2]
输出：[[],[1],[1,2],[1,2,2],[2],[2,2]]
```

**示例 2：**

```
输入：nums = [0]
输出：[[],[0]]
```

 

**提示：**

- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`

### 题解

和上面的子集问题是一样的，关键在于有重复的元素和去重，和之前那道题思路一样

先排序，如果和前一个一样，就检查有没有用过
