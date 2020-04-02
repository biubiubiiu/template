```c++
void quickSort(vector<int>& arr, int lo, int hi) {
    if(lo >= hi) return;
    int pos = partition(arr, lo, hi);
    quickSort(arr, lo, pos-1);
    quickSort(arr, pos+1, hi);
}
```

partition要做的事情是选择一个枢纽元`pivot`,根据每个元素与`pivot`的相对大小关系，将元素分别放置在`pivot`的两侧

#### 两种不同的partition函数实现

**Lomuto partition**（更常用）

```c++
int partition(vector<int>& arr, int lo, int hi) {
    int pivot = arr[hi];
    i = lo;
    for(int j = lo; j < hi; ++j) {
        if(A[j] < pivot) {
            swap(A[i], A[j]);
            ++i;
        }
    }
    swap(A[i], A[hi]);
    return i;
}
```

**Hoare partition**（《数据结构》书籍使用）

```c++
int partition(vector<int>& arr, int lo, int hi) {
    int mid = (lo+hi)>>1;
    int pivot = arr[mid];
    int i = lo, j = hi;
    while(i<j) {
        while(i<j && A[i]<pivot) ++i;
        while(i<j && A[j]>pivot) --j;
        if(i<j){
            swap(A[i], A[j]);
        }
    }
    return j;
}
```



### 选取枢纽元的优化方式

选取`pivot`时，应该尽可能使得分割后的数组两侧具有相同元素，即`pivot`应该尽可能选择数组的中位数

**三数取中**

比较数组的第一个元素、最后一个元素、中间元素。选择三者的中位数作为枢纽元，并调整它们的相对次数，这样在划分过程中，可以减少一次交换操作