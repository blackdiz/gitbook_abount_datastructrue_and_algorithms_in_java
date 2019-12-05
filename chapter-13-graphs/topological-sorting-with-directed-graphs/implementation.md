# Implementation

如前所述，topological sorting 的演算法有 2 個步驟：

1. 找出沒有 successor 的 vertex。successor 是順序上連結在 vertex 之後的 vertex，比方 A -&gt; B 的話 B 即為 A 的 successor。
2. 將該 vertex 從 graph 中刪除後把它新增到另一個列表中。

實作時，確認 vertex 有無 successor 的方法是在 adjacency matrix 中檢查 vertex 是否有和其他 vertex 有 edge 相連，如果沒有就表示沒有 successor。

而刪除 vertex 時則是將 vertex 從儲存 vertex 的 array 中刪除，同時把位置在它左方 \(index 較小的方向\) 的 vertex 向右移動以填滿空格。而 adjacency matrix 中也要刪除掉被刪除的 vertex 所在的行列並且把其他的 vertex 向右向下 \(matrix 中的二維 array 都往 index 大的方向\) 移動填補空格。此外，如果是用 adjacency list 的話這個步驟效率會比較好。

#### Java Code

請參照：[https://github.com/blackdiz/datastructrues\_and\_algorithms\_in\_java/blob/master/src/main/java/chapter13/graph/topologicalsort/TopologicalSort.java](https://github.com/blackdiz/datastructrues_and_algorithms_in_java/blob/master/src/main/java/chapter13/graph/topologicalsort/TopologicalSort.java)



