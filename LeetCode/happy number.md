# [Happy Number](<https://leetcode.com/problems/happy-number/>)

可以证明如果不是happy number，一定存在循环。借鉴链表成环的判断，可以用快慢指针实现happy number的判断。

```c++
class Solution {
public:
    int computesquare(int n){
        int res = 0;
        while(n != 0){
            res += (n%10)*(n%10);
            n = n / 10;
        }
        return res;
    }
    bool isHappy(int n) {
        int slow = n, fast = n;
        do{
            slow = computesquare(slow);
            fast = computesquare(fast); //做两次fast
            fast = computesquare(fast);
        }while(slow != fast); //因为初始化slow=fast，所以要先移动后判断
        return slow == 1;
    }
};
```

