# 栈与队列

## 概念

栈提供push 和 pop 等等接口，所有元素必须符合先进后出规则，所以栈不提供走访功能，也不提供迭代器(iterator)。 不像是set 或者map 提供迭代器iterator来遍历所有元素。

**栈是以底层容器完成其所有的工作，对外提供统一的接口，底层容器是可插拔的（也就是说我们可以控制使用哪种容器来实现栈的功能）。**

所以STL中栈往往不被归类为容器，而被归类为container adapter（容器适配器）。

栈的底层实现可以是vector，deque，list 都是可以的， 主要就是数组和链表的底层实现。

### stack基本操作

- `push()`: 在栈顶添加一个元素。
- `pop()`: 移除栈顶元素。
- `top()`: 返回栈顶元素的引用，但不移除它。
- `empty()`: 检查栈是否为空。
- `size()`: 返回栈中元素的数量。

>- `<stack>` 不提供直接访问栈中元素的方法，只能通过 `top()` 访问栈顶元素。
>- `pop()`不会返回弹出元素的值

### queue基本操作

- `empty()`: 检查队列是否为空。
- `size()`: 返回队列中的元素数量。
- `front()`: 返回队首元素的引用。
- `back()`: 返回队尾元素的引用。
- `push()`: 在队尾添加一个元素。
- `pop()`: 移除队首元素。

### deque基本操作（双端队列）

双端队列是一种允许在两端进行插入和删除操作的线性数据结构。

`<deque>` 的全称是 "double-ended queue"，它在C++中以模板类的形式存在，允许存储任意类型的数据。

`<deque>` 是一个动态数组，它提供了快速的随机访问能力，同时允许在两端进行高效的插入和删除操作。这使得 `<deque>` 成为处理需要频繁插入和删除元素的场景的理想选择。

| 函数名称                               | 功能描述                                          |
| :------------------------------------- | :------------------------------------------------ |
| `deque(size_type n)`                   | 创建一个包含 `n` 个默认值元素的 `deque` 容器。    |
| `deque(size_type n, const T& value)`   | 创建一个包含 `n` 个值为 `value` 的 `deque` 容器。 |
| `assign()`                             | 用新值替换 `deque` 容器中的所有元素。             |
| `at(size_type pos)`                    | 返回 `pos` 位置的元素，并进行范围检查。           |
| `operator[](size_type pos)`            | 返回 `pos` 位置的元素，不进行范围检查。           |
| `front()`                              | 返回第一个元素的引用。                            |
| `back()`                               | 返回最后一个元素的引用。                          |
| `begin()`                              | 返回指向第一个元素的迭代器。                      |
| `end()`                                | 返回指向末尾元素后一位置的迭代器。                |
| `empty()`                              | 检查容器是否为空。                                |
| `size()`                               | 返回容器中的元素个数。                            |
| `max_size()`                           | 返回容器可容纳的最大元素个数。                    |
| `clear()`                              | 清除容器中的所有元素。                            |
| `insert(iterator pos, const T& value)` | 在 `pos` 位置插入 `value` 元素。                  |
| `erase(iterator pos)`                  | 移除 `pos` 位置的元素。                           |
| `push_back(const T& value)`            | 在容器末尾添加 `value` 元素。                     |
| `pop_back()`                           | 移除容器末尾的元素。                              |
| `push_front(const T& value)`           | 在容器前端添加 `value` 元素。                     |
| `pop_front()`                          | 移除容器前端的元素。                              |
| `resize(size_type count)`              | 调整容器大小为 `count`，多出部分用默认值填充。    |
| `swap(deque& other)`                   | 交换两个 `deque` 容器的内容。                     |



## 232.用栈实现队列

### 题目

请你仅使用两个栈实现先入先出队列。队列应当支持一般队列支持的所有操作（`push`、`pop`、`peek`、`empty`）：

实现 `MyQueue` 类：

- `void push(int x)` 将元素 x 推到队列的末尾
- `int pop()` 从队列的开头移除并返回元素
- `int peek()` 返回队列开头的元素
- `boolean empty()` 如果队列为空，返回 `true` ；否则，返回 `false`

**说明：**

- 你 **只能** 使用标准的栈操作 —— 也就是只有 `push to top`, `peek/pop from top`, `size`, 和 `is empty` 操作是合法的。
- 你所使用的语言也许不支持栈。你可以使用 list 或者 deque（双端队列）来模拟一个栈，只要是标准的栈操作即可。

 

**示例 1：**

```
输入：
["MyQueue", "push", "push", "peek", "pop", "empty"]
[[], [1], [2], [], [], []]
输出：
[null, null, null, 1, 1, false]

解释：
MyQueue myQueue = new MyQueue();
myQueue.push(1); // queue is: [1]
myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)
myQueue.peek(); // return 1
myQueue.pop(); // return 1, queue is [2]
myQueue.empty(); // return false
```



 

**提示：**

- `1 <= x <= 9`
- 最多调用 `100` 次 `push`、`pop`、`peek` 和 `empty`
- 假设所有操作都是有效的 （例如，一个空的队列不会调用 `pop` 或者 `peek` 操作）

### 题解

用这道题复习一下栈和队列的`stl`操作

队列是先进来的先出去，栈是后进来的先出去，因此要有两个栈，把栈里的东西倒一下

> C++中stack，其中有两个方法：
>
> pop()，没有返回值，返回void
>
> top()，返回栈顶的引用。
>
> 所以想要提取栈顶元素，直接用s.top()

```c++
class MyQueue {
public:
    stack<int> stIn;
    stack<int> stOut;

    MyQueue() {
        
    }
    
    void push(int x) {
        stIn.push(x);
    }
    
    int pop() {
        if(stOut.empty()){//如果已经颠倒过的栈是空的
        //把没颠倒的颠一下
        while(!stIn.empty()){
            stOut.push(stIn.top());
            stIn.pop();
        }
        }
        int tmp=stOut.top();
        stOut.pop();
        return tmp;
        
    }
    
    int peek() {
        if(stOut.empty()){//如果已经颠倒过的栈是空的
        //把没颠倒的颠一下
        while(!stIn.empty()){
            stOut.push(stIn.top());
            stIn.pop();
        }
        }
        return stOut.top();
    }
    
    bool empty() {
        if(stIn.empty()&&stOut.empty())return true;
        else return false;
    }
};
```

## 225. 用队列实现栈

[力扣题目链接](https://leetcode.cn/problems/implement-stack-using-queues/)

### 题目

请你仅使用两个队列实现一个后入先出（LIFO）的栈，并支持普通栈的全部四种操作（`push`、`top`、`pop` 和 `empty`）。

实现 `MyStack` 类：

- `void push(int x)` 将元素 x 压入栈顶。
- `int pop()` 移除并返回栈顶元素。
- `int top()` 返回栈顶元素。
- `boolean empty()` 如果栈是空的，返回 `true` ；否则，返回 `false` 。

 

**注意：**

- 你只能使用队列的标准操作 —— 也就是 `push to back`、`peek/pop from front`、`size` 和 `is empty` 这些操作。
- 你所使用的语言也许不支持队列。 你可以使用 list （列表）或者 deque（双端队列）来模拟一个队列 , 只要是标准的队列操作即可。

 

**示例：**

```
输入：
["MyStack", "push", "push", "top", "pop", "empty"]
[[], [1], [2], [], [], []]
输出：
[null, null, null, 2, 2, false]

解释：
MyStack myStack = new MyStack();
myStack.push(1);
myStack.push(2);
myStack.top(); // 返回 2
myStack.pop(); // 返回 2
myStack.empty(); // 返回 False
```

 

**提示：**

- `1 <= x <= 9`
- 最多调用`100` 次 `push`、`pop`、`top` 和 `empty`
- 每次调用 `pop` 和 `top` 都保证栈不为空

### 题解

同样是一个熟悉操作和性质的题，利用队列实现栈，与上面那道题很相似但是略有不同，栈入栈后再出栈，可以 使得元素顺序反过来，但是队列无论进出多少次都是一样的顺序，所以只能用一个作为备份，把另外的全部倒出来然后输出最后进去的，再把备份的还进去

```c++
class MyStack {
public:
    queue<int> aq;
    queue<int> bq;
    MyStack() {
        
    }
    
    void push(int x) {
        aq.push(x);
    }
    
    int pop() {
        int size=aq.size();
        int tmp;
        while(size>1){
            tmp=aq.front();
            bq.push(tmp);
            aq.pop();
            size--;
        }
        int ans=aq.front();
        aq.pop();
        while(!bq.empty()){
            int tmp=bq.front();
            aq.push(tmp);
            bq.pop();
        }
        return ans;
    }
    
    int top() {
        int size=aq.size();
        int tmp;
        while(size>=1){
            tmp=aq.front();
            bq.push(tmp);
            aq.pop();
            size--;
        }
        int ans=tmp;
        while(!bq.empty()){
            int tmp=bq.front();
            aq.push(tmp);
            bq.pop();
        }
        return ans;
    }
    
    bool empty() {
        return aq.empty();
    }
};

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack* obj = new MyStack();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->top();
 * bool param_4 = obj->empty();
 */
```

## 20. 有效的括号

==简单== 5min

### 题目

[力扣题目链接](https://leetcode.cn/problems/valid-parentheses/)                                                                                                                                                                                    

给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串 `s` ，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。
3. 每个右括号都有一个对应的相同类型的左括号。

 

**示例 1：**

**输入：**s = "()"

**输出：**true

**示例 2：**

**输入：**s = "()[]{}"

**输出：**true

**示例 3：**

**输入：**s = "(]"

**输出：**false

**示例 4：**

**输入：**s = "([])"

**输出：**true

 

**提示：**

- `1 <= s.length <= 104`
- `s` 仅由括号 `'()[]{}'` 组成

### 题解

括号匹配ez

```c++
class Solution {
public:
    bool isValid(string s) {
        stack<char> bra;
        for(char x:s){
            if(x=='('||x=='{'||x=='[')bra.push(x);
            else if(x==')'){
                if(bra.empty())return false;
                if(bra.top()=='(')bra.pop();
                else return false;
            }
            else if(x=='}'){
                if(bra.empty())return false;
                if(bra.top()=='{')bra.pop();
                else return false;
            }
            else if(x==']'){
                if(bra.empty())return false;
                if(bra.top()=='[')bra.pop();
                else return false;
            }
        }
        return bra.empty();
    }
};
```

看了题解后发现人家写得更好哇，如果是左括号直接把右括号入栈，看结果是否相等就行

```c++
class Solution {
public:
    bool isValid(string s) {
        if (s.length() % 2) { // s 长度必须是偶数
            return false;
        }
        stack<char> st;
        for (char c : s) {
            if (c == '(') {
                st.push(')'); // 入栈对应的右括号
            } else if (c == '[') {
                st.push(']');
            } else if (c == '{') {
                st.push('}');
            } else { // c 是右括号
                if (st.empty() || st.top() != c) {
                    return false; // 没有左括号，或者左括号类型不对
                }
                st.pop(); // 出栈
            }
        }
        return st.empty(); // 所有左括号必须匹配完毕
    }
};

```

##  **1047. 删除字符串中的所有相邻重复项** 

==简单== 5min 

### 题目

给出由小写字母组成的字符串 `s`，**重复项删除操作**会选择两个相邻且相同的字母，并删除它们。

在 `s` 上反复执行重复项删除操作，直到无法继续删除。

在完成所有重复项删除操作后返回最终的字符串。答案保证唯一。

 

**示例：**

```
输入："abbaca"
输出："ca"
解释：
例如，在 "abbaca" 中，我们可以删除 "bb" 由于两字母相邻且相同，这是此时唯一可以执行删除操作的重复项。之后我们得到字符串 "aaca"，其中又只有 "aa" 可以执行重复项删除操作，所以最后的字符串为 "ca"。
```

 

**提示：**

1. `1 <= s.length <= 105`
2. `s` 仅由小写英文字母组成。

### 题解

栈的经典应用。 

要知道栈为什么适合做这种类似于爱消除的操作，因为栈帮助我们记录了遍历数组当前元素时候，前一个元素是什么。

这里还有一些string的操作，可以熟悉一下

```c++
class Solution {
public:
    string removeDuplicates(string s) {
        string result;
        for (char x : s) {
            if (!result.empty() && result.back() == x) {
                result.pop_back();
            } else {
                result.push_back(x);
            }
        }
        return result;
    }
};
```



