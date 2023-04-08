```cpp
void quickSort(vector<int>& arr, int lo, int hi) {
    if (lo >= hi) return;
    int pos = partition(arr, lo, hi);
    quickSort(arr, lo, pos - 1);
    quickSort(arr, pos + 1, hi);
}
```

`partition`要做的事情是选择一个枢纽元`pivot`，根据每个元素与`pivot`的相对大小关系，将元素分别放置在`pivot`的两侧。

#### 两种不同的`partition`函数实现（以递增序为例）

**Lomuto partition**（更常用）

```cpp
int partition(vector<int>& arr, int lo, int hi) {
    int pivot = arr[hi];
    int i = lo;
    for (int j = lo; j < hi; ++j) {
        if (arr[j] < pivot) {
            swap(arr[i], arr[j]);
            ++i;
        }
    }
    swap(arr[i], arr[hi]);
    return i;
}
```

**Hoare partition**（《数据结构》书籍使用）

```cpp
int partition(vector<int>& arr, int lo, int hi) {
    int pivot = nums[start];
    int i = start - 1, j = end + 1;
    while (true) {
        do {
            ++i;
        } while (nums[i] < pivot);
        do {
            --j;
        } while (nums[j] > pivot);

        if (i >= j) return j;
        swap(nums, i, j);
    }
}
```

> Lomoto Parition 和 Hoare Partition 的时间复杂度都为 $O(n)$，但两种方法的一个重要区别是：Lomoto Partition 不仅给出数组划分位置，还将枢纽元`pivot`放置在正确的位置上，而 Hoare Partition 仅给出数组划分位置。

### 枢纽元的优化选取

选取`pivot`时，应该尽可能使得分割后的数组两侧具有相同元素，即`pivot`应该尽可能选择数组的中位数。

**三数取中**

比较数组的第一个元素、最后一个元素和中间元素，选择三者的中位数作为枢纽元，并调整它们的相对次序，这样在划分过程中，可以减少一次交换操作。