Boyer-Moore 是字符串匹配算法的一种；类似于 KMP 算法，它通过对模式串集进行预处理，从而在匹配过程中实现多步后移，以实现更高效的匹配。

算法首先将字符串与模式串头部对齐，<u>从最后一位开始比较</u>。对于不匹配的字符，称之为**坏字符** ，并根据如下规则移动模式串：

* `坏字符规则：后移位数 = 坏字符在模式串中的位置 - 坏字符在模式串中上一次出现的位置`（如果坏字符不包含在模式串中，则上一次出现的位置设为-1）

例如：

![](https://www.ruanyifeng.com/blogimg/asset/201305/bg2013050310.png)

在比较到模式串第 $2$ 位时发现了坏字符 `I`，根据坏字符规则，模式串应后移 $2-(-1)=3$ 位，如下：

![](https://www.ruanyifeng.com/blogimg/asset/201305/bg2013050311.png)

之后，重新从后往前比较模式串和字符串。重复上述步骤，直到结束。

```cpp
constexpr int NO_OF_CHARS = 256;

// The preprocessing function for Boyer Moore's bad character heuristic
void badCharHeuristic(string str, int size, int badchar[NO_OF_CHARS]) {
    int i;

    // Initialize all occurrences as -1
    for (i = 0; i < NO_OF_CHARS; i++) badchar[i] = -1;

    // Fill the actual value of last occurrence of a character
    for (i = 0; i < size; i++) badchar[(int)str[i]] = i;
}

void search(string txt, string pat) {
    int m = txt.size(), n = pat.size();

    int badchar[NO_OF_CHARS];
    badCharHeuristic(pat, n, badchar);

    int s = 0;  // shift of the pattern with respect to text
    while (s <= (n - m)) {
        int j = m - 1;  // try match from the end of pattern
        while (j >= 0 && pat[j] == txt[s + j]) {
            --j;
        }

        // find match
        if (j < 0) {
            cout << "pattern occurs at shift = " << s << endl;
            s += (s + m < n) ? m - badchar[txt[s + m]] : 1;
        } else {
            s += max(1, j - badchar[txt[s + j]]);
        }
    }
}

```

算法的最坏时间复杂度为 $O(mn)$（输入形如txt="AAAAA", pat="AAA"）

