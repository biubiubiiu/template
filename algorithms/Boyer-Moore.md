Boyer-Moore是字符串匹配算法的一种，它需要在O(m)时间内对模式串进行预处理，并在O(n)时间内完成串匹配

首先，将字符串与模式串头部对齐，<u>从最后一位开始比较</u>。对于不匹配的字符，称之为“坏字符”，并根据如下规则移动模式串：

* 坏字符规则：后移位数=坏字符的位置-坏字符在模式串中<u>上一次出现</u>的位置（如果坏字符不包含在模式串中，则上一次出现的位置设为-1）

之后可以得到如下算法：

```java
void badCharHeuristic(char[] str, int size, int[] badchar) {
    int i;
    // Initialize all occurences as -1
    for(i = 0; i < badchar.length; i++) {
        badchar[i] = -1;
    }
    // Record the last occurence of bad char
  	for(i = 0; i < size; i++) {
        badchar[(int)str[i]] = i;
    }
}

void search(char[] txt, char[] pat) {
    int m = pat.length;
    int n = txt.length;
    int[] badchar = new int[NO_OF_CHARS];
    
    // Preprocessing
    badCharHeuristic(pat, m, badchar);
    
    int i = 0;
    while(i <= (n-m)) {
        int j = m-1;
        // match pattern and text backwards
        while(j >= 0 && pat[j] == txt[i+j])
            --j;
        // if we meet bad chars
       	if(j < 0) {
            i += (i+m < n) ? m-badchar[txt[i+m]] : 1;
        } else {
            // the max function is used to ensure a positive shift
            i += Math.max(1, j-badr[txt[s+j]]);
        }
    }
}
```

考虑如下一种情况：

![](https://www.ruanyifeng.com/blogimg/asset/201305/bg2013050310.png)

在比较到模式串第2位时发现了坏字符，根据坏字符规则，模式串应后移2-(-1)=3位，如下：

![](https://www.ruanyifeng.com/blogimg/asset/201305/bg2013050311.png)

考虑模式串已经匹配的"MPLE"四位，我们希望利用这部分信息来得到更好的移法。对于模式串和字符串已经匹配的部分，称之为好后缀，上面就包含"MPLE", "PLE", "LE", "E" 四个好后缀，并根据如下规则移动模式串：

* 好后缀规则：后移位数=好后缀的位置-好后缀在搜索词中上一次出现的位置

这里好后缀的位置定义为最后一个字符的位置，比如"EXAMPLE"中"MPLE"是好后缀，其位置即最后一个字符"E"的位置，为6（与下标一致，从0开始计算）。该好后缀在模式串中上一次出现的位置为-1（未出现）

如果好后缀有多个，则优先选择靠近头部出现的好后缀计算（使后移尽可能小），如在"EXAMPLE"例子中，应选择"E"计算偏移：后移位数=6-0=6，最后效果如下：

![](https://www.ruanyifeng.com/blogimg/asset/201305/bg2013050312.png)

Boyer-Moore算法同时考虑好后缀规则和坏字符规则时，并且每次后移者两个规则的较大值

算法的最坏时间复杂度为O(mn)（输入形如txt="AAAAA", pat="AAA"）

