Dijkstra 算法用于寻找**非负权图上的单源最短路径**，其过程是进行多轮迭代，每次迭代中贪心地选择一个当前的最近节点进行「松弛」操作，以更新起点到各个顶点路径长度，并记录每个顶点的前驱；当最短路径当所有点的路径长度不再变化时，算法完成，此时得到的到所有点的路径长度均为最短路径长度。

```
function dijkstra(G, S)
    for each vertex V in G
        distance[V] <- infinite
        previous[V] <- NULL
        distance[S] <- 0

        U <- the vertex V with minimal distance[V]
        for each unvisited neighbour V of U
            tempDistance <- distance[U] + edge_weight(U, V)
            if tempDistance < distance[V]
                distance[V] <- tempDistance
                previous[V] <- U
    return distance[], previous[]
```

时间复杂度： $O(V^2)$

空间复杂度： $O(V)$

上述平方时间复杂度的原因是需要遍历所有顶点`V`来确定当前的最近顶点，这可以使用有序的数据结构来优化，如二叉堆。使用二叉堆时，可以在 $O(1)$ 时间内寻找到当前最近顶点，而松弛操作会带来 $O(E)$ 次插入操作以及 $O(V)$ 次删除操作，而每次操作的时间复杂度均为 $O(logV)$。因此，使用二叉堆优化后的时间复杂度可以降为 $O((E+V)log(V))$。

---

**地图分析** [Leetcode 1162](https://leetcode-cn.com/problems/as-far-from-land-as-possible/)

（在<u>网格图</u>上运行最短路径算法）

```cpp
class Solution {
   public:
    static constexpr int MAX_N = ...;  // number of vertexs
    static constexpr int INF = INT_MAX;
    static constexpr int dx[4] = {0, 0, -1, 1}, dy[4] = {-1, 1, 0, 0};

    int dis[MAX_N][MAX_N];

    struct Node {
        int dis, x, y;
        bool operator<(const Node &rhs) const { return dis > rhs.dis; }
    };

    priority_queue<Node> q;

    void dijkstra(vector<vector<int>> &grid) {
        int n = grid.size();

        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j]) {
                    dis[i][j] = 0;
                    q.push({0, i, j});
                } else {
                    dis[i][j] = INF;
                }
            }
        }

        while (!q.empty()) {
            auto f = q.top();
            q.pop();
            for (int i = 0; i < 4; ++i) {
                int nx = f.x + dx[i], ny = f.y + dy[i];
                if (!(nx >= 0 && nx <= n - 1 && ny >= 0 && ny <= n - 1))
                    continue;
                if (f.dis + 1 < dis[nx][ny]) { // relax
                    dis[nx][ny] = f.dis + 1;
                    q.push({dis[nx][ny], nx, ny});
                }
            }
        }
    }
};
```
