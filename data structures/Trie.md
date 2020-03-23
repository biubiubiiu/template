Trie: 一种树数据结构，用于检索字符串数据集中的键

Trie 树的结点具有如下字段

* 最多R个指向子结点的链接，其中每个链接对应字母表数据集中的一个字母（假定26）
* 布尔字段，以指定节点是对应键的结尾还是只是键前缀

Trie 树的插入和查找操作时间复杂度均为O(m)，其中m为键长

> 相比之下，AVL树中的查找需要O(mlogn)的时间复杂度

```cpp
class Trie {
private:
    bool is_end = false;
	Trie *next[26] = { nullptr };
public:
    Trie(){}
    
    /** insert a word into the trie **/
    void insert(const string& word) {
        Trie *root = this;
        for(const auto& w: word) {
            if(root->next[w-'a'] == nullptr)
                root->next[w-'a'] = new Trie();
            root = root->next[w-'a'];
        }
        root->is_end = true;
    }
    
    /** Returns if the word is in the trie. */
    bool search(const string& word) {
        Trie *root = this;
        for(const auto& w: word) {
            if(root->next[w-'a'] == nullptr)
                return false;
            root = root->next[w-'a'];
        }
        return root->is_end;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        Trie *root = this;
        for(const auto& p: prefix) {
            if(root->next[p-'a'] == nullptr)
                return false;
            root = root->next[p-'a'];
        }
        return true;
    }
}
```





**Multi Search LCCI** [link](https://leetcode-cn.com/problems/multi-search-lcci/)

给定一个较长字符串`big`和一个包含较短字符串的数组`smalls`，设计一个方法，根据`smalls`中的每一个较短字符串，对`big`进行搜索。输出`smalls`中的字符串在`big`里出现的所有位置`positions`，其中`positions[i]`为`smalls[i]`出现的所有位置。

示例：

>  **输入**：
> big = "mississippi"
> smalls = ["is","ppi","hi","sis","i","ssippi"]
> **输出**： [[1,4],[8],[],[3],[1,4,7,10],[5]]

提示：

* 0 <= len(big) <= 1000
* 0 <= len(smalls[i]) <= 1000
* smalls的总字符数不会超过 100000。
* 你可以认为smalls中没有重复字符串。
* 所有出现的字符均为英文小写字母。

**Solution**:

* option1: 及那个较大字符串的每个后缀都插入trie
* option2：将每个较小的字符串插入到trie中，在trie搜索较大字符串的所有后缀