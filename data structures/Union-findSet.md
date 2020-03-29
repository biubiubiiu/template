```java
class DSU {
    int[] parent;
    
    DSU(int N) {
        parent = new int[N];
        for(int i = 0; i < N; i++) {
            parent[i] = i;
        }
    }
    
    public int find(int x) {
        if(parent[x] == x) return x;
        else {
            parent[x] = find(parent[x]); // 路径压缩
            return parent[x];
        }
    }
    
    public void union(int x, int y) {
        int xr = find(x);
        int yr = find(y);
        parent[xr] = yr;
    }
}
```

