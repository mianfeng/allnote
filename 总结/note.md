# 旨在收集碰到的一些较好的写法

1.**大小写**
```c++
   // 将大写字符转换为小写字符
    std::transform(s.begin(), s.end(), s.begin(), ::tolower);
   
   // 移除非字母数字字符
   s.erase(std::remove_if(s.begin(), s.end(),[](char c) { return !std::isalnum(c); }),s.end());
   ```

3. **KMP算法的C++实现**

```C++

class Solution {
public:
    int strStr(string s, string p) {
        int n = s.size(), m = p.size();
        if(m == 0) return 0;
        //设置哨兵
        s.insert(s.begin(),' ');
        p.insert(p.begin(),' ');
        vector<int> next(m + 1);
        //预处理next数组
        for(int i = 2, j = 0; i <= m; i++){
            while(j and p[i] != p[j + 1]) j = next[j];
            if(p[i] == p[j + 1]) j++;
            next[i] = j;
        }
        //匹配过程
        for(int i = 1, j = 0; i <= n; i++){
            while(j and s[i] != p[j + 1]) j = next[j];
            if(s[i] == p[j + 1]) j++;
            if(j == m) return i - m;
        }
        return -1;
    }
};

```
3. **lamdba表达式的知识点**
![image](https://github.com/mianfeng/allnote/assets/64387330/edd382b9-3c55-46da-b7d0-9deb745604e1)

