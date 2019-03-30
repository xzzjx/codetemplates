# [First unique character in a string](<https://leetcode.com/problems/first-unique-character-in-a-string/> )

频率表，总共26个字母

```c++
class Solution {
public:
    int firstUniqChar(string s) {
        int n = s.size();
        vector<int> freq(26, 0);
        for(int i = 0; i < n; i++){
            freq[s[i] - 'a']++;
        }
        for(int i = 0; i < n; i++){
            if(freq[s[i] - 'a'] == 1) return i;
        }
        return -1;
    }
};
```

