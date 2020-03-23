**最小k个数** [link](https://leetcode-cn.com/problems/smallest-k-lcci/)

找出数组中最小的k个数，以任意顺序返回

示例：

> **输入：** arr = [1, 3, 5, 7, 2, 4, 6, 8], k = 4
>
> **输出：** [1, 2, 3, 4]



**Solution**

使用快速选择算法，该算法与快速排序算法相似。

在快速排序中，每次选取一个枢纽元，并递归地对数组的两部分进行划分。在快速选择中，每次只对数组的一侧进行划分。

算法需要在每次划分后，记录枢纽元的下标`pos`，那么枢纽元应为子数组中第`pos-start+1`小的元素（假设从小到大排序）。那么分为如下三种情况：

* rank == k：枢纽元正好是最小的第k个元素，而左侧元素都比枢纽元小，因此直接返回
* rank < k：继续寻找数组右侧的最小`k-rank`个元素
* rank > k：缩小数组左侧的寻找范围

```c++
vector<int> smallestK(vector<int>& arr, int k) {
    quickSelect(arr, 0, (int)arr.size()-1, k);
    return vector<int>(arr.begin(), arr.begin()+k);
}

void quickSelect(vector<int>& arr, int start, int end, int k) {
    if(start >= end)	return;
    int pos = partition(arr, start, end);
    int rank = pos-start+1;
    if(rank == k)	return;
    else(rank < k)	quickSelect(arr, pos+1, end, k-rank);
    else quickSelect(arr, start, pos-1, k);
}

int partition(vector<int>&arr, int start, int end) {
	// same with quicksort
}
```

每次划分的时间复杂度为$O(n)$，最好情况下，数组划分是平衡的，而快速选择算法每次都进入数组右侧，
$$
T(n)=O(n)+T(n/2)
=>T(n)=O(n)
$$
最坏情况下，数组划分不平衡，且每次进入数组左侧：
$$
T(n)=O(n)+T(n-1)=>T(n)=O(n^2)
$$
