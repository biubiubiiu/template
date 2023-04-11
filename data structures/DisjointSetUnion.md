并查集是一种用于管理元素所属集合的数据结构，实现为一个森林，其中每棵树表示一个集合，树中的节点表示对应集合中的元素。

其支持两种操作：

- 合并（Union）：合并两个元素所属集合（合并对应的树）
- 查询（Find）：查询某个元素所属集合（查询对应的树的根节点），这可以用于判断两个元素是否属于同一集合

```cpp
class DSU {
   public:
    explicit DSU(size_t size) : parents(size), sizes(size, 1) {
        iota(parents.begin(), parents.end(), 0);
    }

    size_t find_set(size_t x) {
        return parents[x] == x
                   ? x
                   : parents[x] = find_set(parents[x]);  // path compression
    }

    void union_sets(size_t x, size_t y) {
        x = find_set(x);
        y = find_set(y);
        if (x == y) return;

        // attach the tree with the lower rank to the higher one
        if (sizes[x] < sizes[y]) swap(x, y);
        parents[y] = x;
        sizes[x] += sizes[y];
    }

   private:
    vector<size_t> parents;

    // in this implementation, we use the size of the trees as rank
    // another choice is to use the depth of the tree as rank
    vector<size_t> sizes;
};
```

时间复杂度： $O(\alpha(n))$，其中 $\alpha(n)$ 为 [inverse Ackermann function](https://en.wikipedia.org/wiki/Ackermann_function)，为一个增长极其缓慢的函数

空间复杂度： $O(n)$