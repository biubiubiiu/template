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

