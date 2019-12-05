# Minimum Spanning Trees

假設現在要設計一張電路板，我們想要確保用到的線路是最小的，也就是元件間不會有多餘的線路。

所以我們需要一種演算法對任何已連接的元件移除多餘的連接線路，也就是產生一個用最少 edge 數來連接所有 vertex 的graph，這就是 **minimun spanning trees \(MST\)** 。

minimum spanning tress 中 vertex 可以有多種可能的連線組合，而 edge 數 E 永遠是 vertex 數 V - 1，也就是：

$$
E = V -1
$$

我們在 minimum spanning trees 中並不在意 edge 的長度，重點不是在找出最短的實際距離，而是找出最少的 edge 數。

minimum spanning trees 的演算法和搜尋幾乎相同，可以用 depth-first search 或 breadth-first search，這裡我們使用 depth-first search。

事實上執行 depth-first search 並記錄下經過的 edge 後就會產生 minimum spanning trees，所以產生 minimum spanning trees 的步驟和 depth-first search 的差別只在於要記錄下經過的 edge。

depth-first search 可以產生  minimum spanning tree 是因為它會拜訪所有的 vertex，但只會拜訪一次，同時它不會走不必要的 edge，所以它的路線一定是可以連結所有 vertex 的最少 edge 數。

#### Java Code

請參照：[https://github.com/blackdiz/datastructrues\_and\_algorithms\_in\_java/blob/master/src/main/java/chapter13/graph/mst/MinimumSpanningTrees.java](https://github.com/blackdiz/datastructrues_and_algorithms_in_java/blob/master/src/main/java/chapter13/graph/mst/MinimumSpanningTrees.java)

