# Definations

在 graph 中，節點稱為 **vertex** ，而連接 vertex 的線稱作 **edge** ，每條 edge 的兩個終端都接著 vertex。

graph 通常不直接對映實際的問題圖形而只是反映 vertex 和 edge 的連接關係，以地圖為例，vertex 可能表示城市，而 edge 表示連接城市的道路，但 edge 的長度並不反映道路真實距離和方向，而一條 edge 可能就表示連接城市間的數條道路。

#### Adjacency

兩個 vertex 如果有一條 edge 相連就視為彼此相鄰，有時候相鄰的 vertex 稱作很此的鄰居。

#### Paths

**path** 表示 edge 的序列，也就是從一個 vertex 到另一個 vertex 會經過的 vertex。假如由 vertex B 到 vertex J 會經過 vertex A 和 vertex E，那 path 就是 BAEJ，vertex 之間的 path 可能會有多條。

#### Connected Graphs

當每個 vertex 都至少有一條 path 可以到其他 vertex 時，這個 graph 就視為 **connected** \(連通的\)。

而 non-connected graph 中，相連的 vertex 稱為一組 **connected components** \(連通元件\)。為了簡化，此章中討論的演算法都是應用在 connected graph 或 connected component 上，通常透過一些修改後也可以應用在 non-connected graph 上。

#### Directed and Weighted Graphs

如果 edge 沒有方向的話，我們可以從 vertex A 到 vertex B 或反過來由 vertex B 到 vertex A，此時我們稱這個 graph 為 **non-directed graph** \(無向圖\)。

若 graph 的 edge 如果具有方向性，表示只可從 vertex A 往 vertex B，但不能反過來，如同單行道一樣，這時稱為 **directed graph** \(有向圖\)，通常 edge 會以箭頭表示可移動的方向。

有些 graph 中，edge 會被賦予 **weight** \(權重\) 數字，這樣的 graph 稱作 **weighted graph** ，weight 可以表示 vertex 間的實際距離，某個 vertex 到另一個 vertex 的次數，或是移動到另一個 vertex 的成本 \(比方機票價格\)。

