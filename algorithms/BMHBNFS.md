reference: http://effbot.org/zone/stringlib.htm

该算法同时结合了几种串匹配算法的特点，效率极高，也是python3中内置find()函数使用的算法

```python
def find(s, p):
    # find first occurrence of p in s
    n = len(s)
    m = len(p)
    skip = delta1(p)[p[m-1]]
    i = 0
    while i <= n-m:
        if s[i+m-1] == p[m-1]: # (boyer-moore)
            # potential match
            if s[i:i+m-1] == p[:m-1]:
                return i
            if s[i+m] not in p:
                i = i + m + 1 # (sunday)
            else:
                i = i + skip # (horspool)
        else:
            # skip
            if s[i+m] not in p:
                i = i + m + 1 # (sunday)
            else:
                i = i + 1
    return -1 # not found
```

