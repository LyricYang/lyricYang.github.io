[TOC]


## 最小生成树算法
> 给定一个无方向的带权图`G=(V,E)`,最小生成树为集合`T`, `T`是以最小代价连接`V`中所有顶点所用边`E`的最小集合。集合`T`中的边能够形成一颗树，这是因为每个节点（除了根节点）都能向上找到它的一个父节点。

- **连通图**：在无向图中，若任意两个顶点都有路径相通，则称该无向图为连通图。
- **强连通图**：在有向图中，若任意两个顶点都有路径相通，则称该有向图为强连通图。
- **连通网**：在连通图中，若图的边具有一定的意义，每一条边都对应着一个数，称为权；权代表着连接连个顶点的代价，称这种连通图叫做连通网。
- **生成树**：一个连通图的生成树是指一个连通子图，它含有图中全部n个顶点，但只有足以构成一棵树的n-1条边。一颗有n个顶点的生成树有且仅有n-1条边，如果生成树中再添加一条边，则必定成环。
- **最小生成树**：在连通网的所有生成树中，所有边的代价和最小的生成树，称为最小生成树。

![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\最小生成树和最短路径算法\92d4b71b-e582-47f1-8866-897eeabb2161.png)

### Prim算法

> `Prim`算法是一种产生最小生成树的算法。该算法于1930年由捷克数学家沃伊捷赫·亚尔尼克（英语：`Vojtěch Jarník`）发现；并在1957年由美国计算机科学家罗伯特·普里姆（英语：`Robert C. Prim`）独立发现；1959年，`艾兹格·迪科斯彻`再次发现了该算法。

Prim算法又称“加点法”，算法从任意顶点开始，每次选择一个与当前顶点集最近的一个点，加入顶点集中。
1. 图的所有顶点集合为$V$;初始令最小生成树顶点集为$u=\{s\}$,$v=V-u$;
2. 在两个集合$u$和$v$之间能够组成的边中，选择一条代价最小的边$(u_i,v_j)$，加入到最小生成树中，并把$v_j$加入集合$u$中；
3. 重复上述步骤，直到取完图中所有的点。

![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\最小生成树和最短路径算法\e6510d4e-efc3-48e3-bb65-355e32682ee6.jpg)

### Kruskal算法

>克鲁斯卡尔算法是一种用来寻找最小生成树的算法。在剩下的所有未选取的边中，找最小边，如果和已选取的边构成回路，则放弃，选取次小边。

此算法可以称为“加边法”，初始最小生成树边数为0，每迭代一次就选择一条满足条件的最小代价边，加入到最小生成树的边集合里。
1. 把图中的所有边按代价从小到大排序；
2. 把图中的n个顶点看成独立的n棵树组成的森林；
3. 按权值从小到大选择边，所选的边连接的两个顶点,应属于两棵不同的树，则成为最小生成树的一条边，并将这两棵树合并作为一棵树。
4. 重复(3),直到所有顶点都在一颗树内或者有n-1条边为止。

![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\最小生成树和最短路径算法\706f13e9-3a44-4eaf-8930-48598aa38680.jpg)

```java
public class KruskalMST {

    public static void main(String[] args){
        int[][] Graph = {{0, 6, 1, 5, Integer.MAX_VALUE, Integer.MAX_VALUE},
                {6, 0, 5, Integer.MAX_VALUE, 3, Integer.MAX_VALUE},
                {1, 5, 0, 5, 6, 4},
                {5,Integer.MAX_VALUE,5,0,Integer.MAX_VALUE,2},
                {Integer.MAX_VALUE, 3, 6, Integer.MAX_VALUE, 0, 6},
                {Integer.MAX_VALUE, Integer.MAX_VALUE, 4, 2, 6, 0}};
        char[] V={'A','B','C','D','E','F'};
        int n=6;
        Kruskal(V,Graph,n);
    }

    private static void Kruskal(char[] V,int[][] G,int n){
        int[] arrOfVertex = new int[n];
        int from = 0;
        int to = 0;
        int sum = 0;
        int curNum = 1;
        while (curNum<n){
            int min = Integer.MAX_VALUE;
            for(int i=0;i<n;i++){
                for(int j=i+1;j<n;j++){
                    if(G[i][j]>0 && G[i][j]<min){
                        if(arrOfVertex[i] != arrOfVertex[j] || (arrOfVertex[i] == 0 && arrOfVertex[j] == 0)){
                            min = G[i][j];
                            from = i;
                            to = j;
                        }
                    }
                }
            }

            System.out.println(V[from]+"--"+V[to]+" : "+min);
            sum+=min;

            if(arrOfVertex[from] == 0 && arrOfVertex[to] == 0){
                arrOfVertex[from] = curNum;
                arrOfVertex[to] = curNum;
                curNum++;
            }else if(arrOfVertex[from]==0){
                for(int i=0;i<n;i++){
                    if(arrOfVertex[to]==arrOfVertex[i])
                        arrOfVertex[i]=curNum;
                }
                arrOfVertex[from] = curNum;
                arrOfVertex[to] = curNum;
                curNum++;
            }else if(arrOfVertex[to]==0){
                for(int i=0;i<n;i++){
                    if(arrOfVertex[from]==arrOfVertex[i])
                        arrOfVertex[i]=curNum;
                }
                arrOfVertex[from] = curNum;
                arrOfVertex[to] = curNum;
                curNum++;
            }else {
                for(int i=0;i<n;i++){
                    if(arrOfVertex[from]==arrOfVertex[i])
                        arrOfVertex[i]=curNum;
                }
                for(int i=0;i<n;i++){
                    if(arrOfVertex[to]==arrOfVertex[i])
                        arrOfVertex[i]=curNum;
                }
                curNum++;
            }
        }
        System.out.println("Sum : "+sum);
    }
}
```

## 最短路径算法

### Dijkstra算法
>迪杰斯特拉算法，是一个经典的最短路径算法，主要特点是以起始点为中心向外层层扩展，直到扩展到终点为止，使用了广度优先搜索解决赋权有向图的单源最短路径问题，算法最终得到一个最短路径树。时间复杂度为O(N^2)

![](https://img-blog.csdn.net/20170129054035439?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamVycnk4MTMzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

**单源最短路径：**在无向图`G=(V,E)`中，假设每条边`E[i]`的长度为`w[i]`，找到由顶点`V0`到其余各点的最短路径。

**算法思想：**设G=(V,E)是一个带权有向图，把图中顶点集合V分成两组，第一组为已求出最短路径的顶点集合（用S表示，初始时S中只有一个源点，以后每求得一条最短路径 , 就将加入到集合S中，直到全部顶点都加入到S中，算法就结束了），第二组为其余未确定最短路径的顶点集合（用U表示），按最短路径长度的递增次序依次把第二组的顶点加入S中。在加入的过程中，总保持从源点v到S中各顶点的最短路径长度不大于从源点v到U中任何顶点的最短路径长度。此外，每个顶点对应一个距离，S中的顶点的距离就是从v到此顶点的最短路径长度，U中的顶点的距离，是从v到此顶点只包括S中的顶点为中间顶点的当前最短路径长度。

**算法步骤：**

1. 初始时，S只包含源点，即S＝{v}，v的距离为0。U包含除v外的其他顶点，即:U={其余顶点}，若v与U中顶点u有边，则<u,v>正常有权值，若u不是v的出边邻接点，则<u,v>权值为∞。
2. 从U中选取一个距离v最小的顶点k，把k，加入S中（该选定的距离就是v到k的最短路径长度）。
3. 以k为新考虑的中间点，修改U中各顶点的距离；若从源点v到顶点u的距离（经过顶点k）比原来距离（不经过顶点k）短，则修改顶点u的距离值，修改后的距离值的顶点k的距离加上边上的权。
4. 重复步骤b和c直到所有顶点都包含在S中。

![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\最小生成树和最短路径算法\e7291c8e-77fe-4eb3-b9a7-0c984af27069.jpg)

```java
public class DijkstraTest {
    public static void main(String[] args){
        int[][] Graph = {{0, 7, 9, Integer.MAX_VALUE, Integer.MAX_VALUE,14},
                {7, 0, 10,15, Integer.MAX_VALUE,Integer.MAX_VALUE},
                {9, 10, 0, 11, Integer.MAX_VALUE, 2},
                {Integer.MAX_VALUE,15,11,0,6,Integer.MAX_VALUE},
                {Integer.MAX_VALUE, Integer.MAX_VALUE,Integer.MAX_VALUE,6,0,9},
                {14, Integer.MAX_VALUE, 2,Integer.MAX_VALUE, 9, 0}};
        int n=6;
        Dijkstra(Graph,0,n);
    }

    public static void Dijkstra(int[][] G,int v0,int n){
        boolean[] S = new boolean[n];
        int[] dist = new int[n];
        int[] prev = new int[n];
        for(int i=0;i<n;i++) {
            dist[i] = G[v0][i];
            S[i] = false;
            if (dist[i] == Integer.MAX_VALUE) {
                prev[i] = -1;
            } else {
                prev[i] = v0;
            }
        }
        S[v0]=true;
        for(int i=1;i<n;i++){
            int minDist = Integer.MAX_VALUE;
            int u=v0;
            for (int j = 0; j < n; j++) {
                if((!S[j]) && dist[j]<minDist){
                    u=j;
                    minDist=dist[j];
                }
            }
            S[u]=true;
            for(int j=0;j<n;j++) {
                if ((!S[j]) && G[u][j] < Integer.MAX_VALUE) {
                    if (dist[u] + G[u][j] < dist[j]) {
                        dist[j] = dist[u] + G[u][j];
                        prev[j] = u;
                    }
                }
            }
        }
        for(int i=0;i<n;i++){
            System.out.print(i+" ");
        }
        System.out.println();
        for(int i=0;i<n;i++){
            System.out.print(dist[i]+" ");
        }
    }
}
```


### Floyd算法
> 此算法由Robert W. Floyd（罗伯特·弗洛伊德）于1962年发表在“Communications of the ACM”上。同年Stephen Warshall（史蒂芬·沃舍尔）也独立发表了这个算法。Robert W．Floyd这个牛人是朵奇葩，他原本在芝加哥大学读的文学，但是因为当时美国经济不太景气，找工作比较困难，无奈之下到西屋电气公司当了一名计算机操作员，在IBM650机房值夜班，并由此开始了他的计算机生涯。

Floyd算法可以求解任意两点间的最短路径的长度。

![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\最小生成树和最短路径算法\Floyd1528700734395.png)

```java
public class FloydTest {
    public static void main(String[] args){
        int[][] Graph = {{0, 7, 9, Integer.MAX_VALUE, Integer.MAX_VALUE,14},
                {7, 0, 10,15, Integer.MAX_VALUE,Integer.MAX_VALUE},
                {9, 10, 0, 11, Integer.MAX_VALUE, 2},
                {Integer.MAX_VALUE,15,11,0,6,Integer.MAX_VALUE},
                {Integer.MAX_VALUE, Integer.MAX_VALUE,Integer.MAX_VALUE,6,0,9},
                {14, Integer.MAX_VALUE, 2,Integer.MAX_VALUE, 9, 0}};
        char[] V={'A','B','C','D','E','F'};
        int n=6;
        Floyd(V,Graph,n);
    }

    private static void Floyd(char[] v, int[][] graph, int n) {
        int[][] dist = new int[n][n];

        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                dist[i][j]=graph[i][j];
            }
        }

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                for (int k = 0; k < n; k++) {
                    int temp = (dist[j][i]==Integer.MAX_VALUE||dist[i][k]==Integer.MAX_VALUE)?Integer.MAX_VALUE:dist[j][i]+dist[i][k];
                    if(dist[j][k]>temp){
                        dist[j][k]=temp;
                    }
                }
            }
        }

        System.out.println("Floyd: ");
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                System.out.print(dist[i][j]+" ");
            }
            System.out.println();
        }
    }
}
```