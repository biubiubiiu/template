Sunday算法是字符串匹配算法的一种，其算法如下：

* 目标字符串`String​`
* 模式串`Pattern​`
* 当前查询索引`idx`
* 待匹配字符串`str_part`:  `String[ idx : idx + len(Pattern) ]`

每次匹配都从目标字符串中提取待匹配字符串与模式串匹配

* 若匹配，则返回当前`idx`

* 若不匹配，则查看待匹配字符串后一位`c`:

  * 若`c`存在于`Pattern`中，则`idx=idx+shift[c]`

  * 否则，`idx=idx + len(Pattern)`

* 重复上述步骤，直到`idx+ len(Pattern) > len(String)`

shift（偏移表）的作用是存储每一个在模式串中出现的字符，在**模式串**中出现的**最右位置**到尾部的**距离** +1，如果没有出现，则约定为模式串长度+1

$$
shift[w] = \begin{cases}  
m-max\{i<m|P[i]=w\} & if\ w\ is\ in\ P[0..m-1] \\
m+1 & otherwise
\end{cases}
$$
算法最坏情况下复杂度为：O(mn) ，平均情况下复杂度为O(n)

![Step1](https://pic.leetcode-cn.com/3d311ba9ddbb37487a475b95875f49351750aa89cea6b77d974cde45603cf63d-%E6%88%AA%E5%B1%8F2019-10-0716.15.15.png)

![Step2](https://pic.leetcode-cn.com/c959fe5147959af3cb32a98183ac430a1145ae4341fe7620105ccd5b88d77b59-%E6%88%AA%E5%B1%8F2019-10-0716.16.44.png)

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
    
        # Func: 计算偏移表
        def calShiftMat(st):
            dic = {}
            for i in range(len(st)-1,-1,-1):
                if not dic.get(st[i]):
                    dic[st[i]] = len(st)-i
            dic["ot"] = len(st)+1
            return dic
        
        # 其他情况判断
        if len(needle) > len(haystack):return -1
        if needle == "": return 0
       
        # 偏移表预处理    
        dic = calShiftMat(needle)
        idx = 0
    
        while idx+len(needle) <= len(haystack):
            
            # 待匹配字符串
            str_cut = haystack[idx:idx+len(needle)]
            
            # 判断是否匹配
            if str_cut==needle:
                return idx
            else:
                # 边界处理
                if idx+len(needle) >= len(haystack):
                    return -1
                # 不匹配情况下，根据下一个字符的偏移，移动idx
                cur_c = haystack[idx+len(needle)]
                if dic.get(cur_c):
                    idx += dic[cur_c]
                else:
                    idx += dic["ot"]
            
        
        return -1 if idx+len(needle) >= len(haystack) else idx
```

```c++
//@ref: https://www.inf.hs-flensburg.de/lang/algorithmen/pattern/sundayen.htm
void sundayInitooc() {
    int j;
    char a;
    for(a = 0; a < alphabetsize; a++)
        occ[a] = -1;
    for(j = 0; j < m; j++) {
        a = p[j];
        occ[a] = j;
    }
}

void sundaySearch() {
    int i = 0;
    while(i <= n-m) {
        if(matchesAt(i))	report(i);
        i += m;
        if(i < n) i -= occ[t[i]];
    }
}
```

