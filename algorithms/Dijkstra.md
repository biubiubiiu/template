Leetcode 1162.地图分析 [link](https://leetcode-cn.com/problems/as-far-from-land-as-possible/)

在<u>网格图</u>上运行的最短路径算法

Dijkstra 算法的堆优化版本，时间复杂度为O(ElogV)​

```c++
class Solution {
public:
    static constexpr int MAX_N = ...; // number of vertexs
    static constexpr int INF = INT_MAX;
    static constexpr int dx[4] = {0, 0, -1, 1}, dy[4] = {-1, 1, 0, 0};
    
    int n;
    int dis[MAX_N][MAX_N];
    
    struct Status {
        int v, x, y;
        bool operator < (const Status &rhs) const {
            return v > rhs.v;
        }
    }
    
    priority_queue<Status> q;
    
    void Dijkstra(vector<vector<int>>& grid) {
        this->n = grid.size();
        auto &a = grid;
        
        for(int i = 0; i < n; ++i) {
            for(int j = 0; j < n; ++j) {
                dis[i][j] = INF;
            }
        }
        
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (a[i][j]) {
                    d[i][j] = 0;
                    q.push({0, i, j}); // 添加源节点
                }
            }
        }
        
        while (!q.empty()) {
            auto f = q.top(); q.pop();
            for (int i = 0; i < 4; ++i) {
                int nx = f.x + dx[i], ny = f.y + dy[i];
                if (!(nx >= 0 && nx <= n - 1 && ny >= 0 && ny <= n - 1)) continue;
                if (f.v + 1 < d[nx][ny]) {
                    d[nx][ny] = f.v + 1;
                    q.push({d[nx][ny], nx, ny});
                }
            }
        }
    }
}
```

