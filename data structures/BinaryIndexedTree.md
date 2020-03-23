![](https://img2018.cnblogs.com/blog/1448672/201810/1448672-20181003121604644-268531484.png)

* C[1] = A[1]
* C[2] = A[1] + A[2]
* C[3] = A[3]
* C[4] = A[1] + A[2] + A[3] + A[4]
* ...
* $C[i]=A[i-2^k+1]+A[i-2^k+2]+...+A[i]，k为i的二进制中从最低位到高位连续零的长度$

```c++
int lowbit(int x) {
    return x & (-x);
}

int getSum(int BITree[], int index) 
{ 
	int sum = 0;
    
    // Traverse ancestors of BITree[index] 
    for(index = index+1; index > 0; index -= lowbit(index)) {
        sum += BITree[index]; 
    }
	return sum; 
} 

// Updates a node in Binary Index Tree (BITree) at given index 
// in BITree. The given value 'val' is added to BITree[i] and 
// all of its ancestors in tree. 
void updateBIT(int BITree[], int n, int index, int val) 
{ 
	for(index = index+1; index <= n; index += lowbit(index)) {
        BITree[index] += val;
    }
} 

// Constructs and returns a Binary Indexed Tree for given 
// array of size n. 
int *constructBITree(int arr[], int n) 
{ 
	// Create and initialize BITree[] as 0 
	int *BITree = new int[n+1]; 
	for (int i=1; i<=n; i++) 
		BITree[i] = 0; 

	// Store the actual values in BITree[] using update() 
	for (int i=0; i<n; i++) 
		updateBIT(BITree, n, i, arr[i]); 

	return BITree; 
} 
```

树状数组的各操作复杂度为：

* 构造：$O(n)$
* 区间查询：$O(logn)$
* 单点修改：$O(logn)$





**使用树状数组求逆序对**

先把数列中的数按大shuzhangs小顺序转化为1到n的整数，使得原数列成为一个1,2,...n的排列P，创建一个树状数组，用来记录数组A（下标从1开始）的前缀和。若排列中的数i当前已经出现，则A[i]的值为1，否则为0。

初始时数组A的值均为0，从排列中的<u>最后一个数开始遍历</u>，每次在树状数组中查询有多少个数小于当前的数P[j] （即用树状数组查询数组A目前P[j]-1个数的前缀和）并加入计数器，之后对树状数组执行修改数组A第P[j]个数值加1的操作。