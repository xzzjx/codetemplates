# [Largest Rectangle in Histogram](<https://leetcode.com/problems/largest-rectangle-in-histogram/>)

## 动态规划
1. 将问题分解为求leftmost和rightmost两个动态规划问题
2. 注意最优子问题结构依旧存在
3. 第i时刻的状态不是依赖于i-1或者之前某些确定的状态，而是以i-1的状态为跳板进行jump。块连续是这个递推关系成立的重要原因。

```c++
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int n = heights.size();
        if(n == 0) return 0;
        int res = 0;
        vector<int> leftmost(n, 0); //记录i左侧height小于i的下标
        vector<int> rightmost(n, 0);
        leftmost[0] = -1;
        rightmost[n-1] = n;
        for(int i = 1; i < n; i++){
            int p = i - 1;
            while(p >= 0 && heights[p] >= heights[i]){
                p = leftmost[p];
            }
            leftmost[i] = p;
        }
        for(int i = n-2; i >= 0; i--){
            int p = i + 1;
            while(p < n && heights[p] >= heights[i]){
                p = rightmost[p];
            }
            rightmost[i] = p;
        }
        for(int i = 0; i < n; i++){
            res = max(res, heights[i] * (rightmost[i] - leftmost[i] - 1));
        }
        return res;
    }
};
```

## 单调栈

<https://zhuanlan.zhihu.com/p/26465701> 

构造一个非递增的栈。遍历数组，当heights[i]小于当前栈顶，则出栈，并计算以当前栈顶为最小高度的长方形面积。

```c++
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        stack<int> s;
        int n = heights.size();
        if(n == 0) return 0;
        s.push(0);
        int res = 0;
        for(int i = 1; i < n; i++){
            while(!s.empty() && heights[i] < heights[s.top()]){
                int cur = s.top();
                s.pop();
                int leftmost = s.empty() ? -1 : s.top();
                res = max(res, heights[cur] * (i - leftmost - 1));
            }
            s.push(i);
        }
        if(!s.empty()){
            int rightmost = s.top(); // 事实上s.top() = n-1
            while(!s.empty()){
                int cur = s.top();
                s.pop();
                int leftmost = s.empty() ? -1 : s.top();
                res = max(res, heights[cur] * (rightmost - leftmost));
            }
        }
        return res;
    }
};
```

## 分治

<https://www.geeksforgeeks.org/largest-rectangular-area-in-a-histogram-set-1/> 