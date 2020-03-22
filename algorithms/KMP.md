`needle`为要匹配的字符串

`next`数组的作用是让算法无需多次匹配`haystack`中的任何字符。能够实现线性时间搜索的关键是在主串的一些字段中检查模式串的初始字段，我们可以确切地知道在当前位置之前的一个潜在匹配的位置。

算法需要O(m)（m为needle的串长度）的复杂度来计算next数组，并实现在O(n)（n为字符串长度）时间内完成串匹配

```java
class Solution {
    public static int[] cal_next(String str) { // 计算关于 模式串 的 next 数组
        int len = str.length(), next[] = new int[len];
        next[0] = -1;
        int i = 0, j = 1; // i,j 分别是前后缀指针
        while(j < len) {
            if(i == -1 || str.charAt(i) == str.charAt(j)) {
                i++;
                j++;
                if(j < len) next[j] = i;
            } else i = next[i];
        }
        return next;
    }

    public static int strStr(String haystack, String needle) {
        if(needle.length() == 0) return 0;
        if(haystack.length() == 0) return -1;
        int[] next = cal_next(needle); // 计算 needle 的前后缀
        int i = 0, j = 0, len = haystack.length(), subLen = needle.length();
        while(i < len) {
            if(j == -1 || haystack.charAt(i) == needle.charAt(j)) {
                i++;
                j++;
                if(j == subLen) return i - j; // needle 指针走到末尾，说明匹配成功
            } else j = next[j];
        }
        return -1;
    }
}
```

​	