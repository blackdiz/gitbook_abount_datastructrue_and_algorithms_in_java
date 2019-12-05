# Efficiency

graph 演算法的時間複雜度依使用的是 adjacency matrix 或  adjacency list 實作而不同。

如果使用 adjacency matrix，大多數的演算法時間複雜度都會是 O\(V ^ 2\)，V 為 vertex 數。因為這些演算法需要每個 vertex 間的 edge 都檢視過，換句話說就是必須檢視 adjacency matrix 的每個欄位，而欄位數會是 vertex 數的平方，所以時間複雜度為 O\(V ^ 2\)。

在數量大的狀況下 O\(V ^ 2\) 的效率不太好，但如果 graph 很密集也就是有許多 edge 時能改進的方法也不多。但許多 graph 的狀態是很鬆散的也就是 vertex 間沒有太多 edge 相連，在較鬆散的 graph 中，使用 adjacency list 實作可以增進效能，因為 list 中只含有和該 vertex 相連的 edge，所以我們需要檢視的 edge 數會比用 adjacency matrix 來得少。

對 unweighted graph 而言，用 adjacency list 實作 depth-first search 的時間複雜度是 O\(V + E\)，V 為 vertex 數，E 為 edge 數。而對 weighted graph 來說，minimum spanning tree 和 shortest-path 的時間複雜度則為 O\(\(E + V\)logV\)。應用在巨大而較鬆散的 graph 上時，這樣的時間複雜度比起用 adjacency matrix 實作有長足的進步，但用 adjacency list 實作時演算法本身的實作難度也會增加。

Warshall 和 Floyd 演算法比起其他演算法來得慢，兩者都需要 O\(V ^ 3\) 因為它需用使用 3 層的巢狀迴圈實作。



