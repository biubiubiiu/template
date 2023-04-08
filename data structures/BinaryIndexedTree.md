![](https://img2018.cnblogs.com/blog/1448672/201810/1448672-20181003121604644-268531484.png)

* $C[1] = A[1]$
* $C[2] = A[1] + A[2]$
* $C[3] = A[3]$
* $C[4] = A[1] + A[2] + A[3] + A[4]$
* ...
* $C[i]=A[i-2^k+1]+A[i-2^k+2]+...+A[i]$，其中 $2^k$ 用于定位 $i+1$ 的二进制表示中`1`的位置

```cpp
class BinaryIndexedTree {
   public:
    // Constructs and returns a Binary Indexed Tree for given
    // vector of size n.
    BinaryIndexedTree(const vector<int>& arr) {
        this->n = arr.size();

        // Create and initialize BITree[] as 0
        items = new int[n + 1];
        for (int i = 1; i <= n; ++i) items[i] = 0;

        // Store the actual values in BITree[] using update()
        for (int i = 0; i < n; i++) update(i, arr[i]);
    }

    ~BinaryIndexedTree() { delete[] items; }

    // Updates a node in Binary Index Tree (BITree) at given
    // index in BITree. The given value 'val' is added to
    // items[i] and all of its ancestors in tree.
    void update(int index, int val) {
        for (int i = index + 1; i <= n; i += lowbit(i)) {
            items[i] += val;
        }
    }

    // Get the sum of elements in 0 to index(inclusive)
    int getSumOfInterval(int index) {
        int result = 0;
        for (int i = index + 1; i > 0; i -= lowbit(i)) {
            result += items[i];
        }
        return result;
    }

   private:
    int* items;
    int n;
    int lowbit(int x) { return x & (-x); }
};
```

树状数组的各操作复杂度为：

* 构造： $O(n)$
* 区间查询： $O(logn)$
* 单点修改： $O(logn)$

---

**使用树状数组求逆序对**

先把数列中的数按大小顺序转化为 $1..n$ 的整数表示（离散化），使得原数列成为一个由 $1,2,...,n$ 组成的排列 $\mathcal{P}$，创建一个树状数组，用来记录数组 $A$（下标从1开始）的前缀和。

初始时 $A[i]=0,\forall i$。之后，从后往前遍历 $\mathcal{P}$。遇到数 $i$ 后将 $A[i]$ 的值置为1，并查询有多少个数小于 $i$（在树状数组中查询前 $i-1$ 个数的前缀和）并加入计数器。