# 无向图

## 常用场景

### 地图

判断两个点是否有路径相通、最短距离是多少，最短路程时间是多少

### 调度

有一些任务要执行，任务之间存在一些依赖限制关系，如何执行能够既满足依赖关系时间又短

## 商业

## 匹配

## 计算机网络

如何有效部署网络线路和交换机

## 软件


## 社交网络

application | item | connection

map | intersection | road

## 1.1 无向图(undirected graph)

edge 边
vertice 点

> A graph is a set of vertices and a collection of edges that each connect a pair of vertices.

树是无环连通图。一些不相连的树称为森林。生成树是一个连通图里的链接所有顶点的子图且是一个树。生成森林是所有连通分量的生成树的总和

graph G有V个顶点，当且仅当如下满足如下5个条件时，它是一个树
1. G有V-1个边，没有环
2. G有V-1个边，并且都是相连的
3. G是连通的，但是删除任何一个边都不再是连通的
4. G是无环的，但是增加任何一个边都会形成一个环
5. 每两个顶点直接有且只有一个简单路径

双色图(bipartite graph)，是一种可以把顶点分成两个集合的图，任何一个顶点之和其他集合中的顶点相连

## 1.2 图的不同表示

* 邻接矩阵: 空间复杂度为O(N^2)，无法表示平行边
* 邻接链表: 更加通用

列表数组表示所有的邻接链表，或者使用Map存储

decouple our implementations from the graph representation

## Depth-first serach

搜索一个图，使用递归的方式访问每一个顶点。
访问一个顶点时，先标记这个顶点已经被访问过了。然后递归地访问这个顶点的没有遍历过的邻接顶点

```
public class DepthFirstSearch {
    private boolean[] marked;
    public DepthFirstSearch(Graph g, int s) {
        marked = new boolean[g.V()];
        dfs(g, s);
    }

    private void dfs(Graph g, int v) {
        marked[start] = true;
        for (int w : G.adj(v)) {
            if (!marked[w]) {
                dfs(g, w);
            }
        }
    }
}
```

tracing DFS

DFS traverse图中的每个边两次，第二次总是会发现一个已经访问过的顶点。

能够用于解决path detectino problem

```
class Paths {
    Paths(Graph g, int s);
    boolean hasPathTo(int v);
    Iterable<Integer> pathTo(int v);
}
```

```
class DepthFirstPaths {
    private boolean[] marked;
    private int[] edgeTo;
    private final S;
    DepthFirstPaths(Graph G, int s) {
        marked = new boolean[G.V()];
        edgeTod = new int[G.V()];
        this.s = s;
        dfs(G, s);
    }

    private void dfs(Graph G, int v) {
        marked[v] = true;
        for (int w : G.adj(v)) {
            if (!marked[w]) {
                edgeTo[w] = v;
                dfs(G, w);
            }
        }
    }

    public boolean hasPathTo(int v) {
        return marked[v];
    }

    public Iterable<Integer> pathTo(int v) {
        if (!hashPathTo(v)) {
            return null;
        }
        Stack<Integer> path = new Stack<Integer>();
        for (int x = v; x != s; x = edgeTo[x]) {
            path.push(x);
        }
        path.push(s);
        return path;
    }
}
```

### Breadth-first search

> Single-source shorted paths. 单源最短路径

in DFS, we use pushdown stack(managed by system to support the recursive search method)

In BFS, we use a (FIFO) queue instead of (LIFO) stack.

We choose, of the passages yet to be explored, the one 
that was least recently encountered

It is based on maintaining a queue of all vertices that have
been marked but whose adjacency lists have not been checked.
We put the source vertex on the queue, then perform the 
folling steps until the queue is empty.
- Take the next vertex v from the queue and mark it
- Put onto the queue all unmarked vertices that are adjacent to v

```
private void bfs(Graph g, int s) {
    Queue<Integer> q = new Queue<Integer>();
    makred[s] = true;
    q.enqueue(s);
    while (!q.isEmpty) {
        int v = q.dequeue();
        for (int w : G.adj(v)) {
            if (!makred[w]) {
                edgeTo[w] = v;
                marked[w] = true;
                queue.enqueue(w);
            }
        }
    }
}
```

### 连通分量(connected components)

维护一个count计数，dfs遍历记录每个顶点的count值，每个分量遍历完count++

### Symbol graphs

有一些情况不能完全用数组来表示。一种解决方案是用一个Map<String, Integer>
保存string到id的映射，或者完全使用Map

Problem | Solution 
single-source connecivity | DFS
single-source paths | DFS
single-source shortest paths | BFS
connectivity | CC
cycle detection | Cycle
two-colorability(bipartiteness) TwoColor