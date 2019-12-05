# Search

graph 的最基本操作之一就是找到從特定的 vertex 可以到哪些其他 vertex，比方我們必須找出從 Kansan 城透過火車可以到哪些城市。

假設我們根據問題建立了一個 graph，現在需要一個演算法可以從特定的 vertex 開始，然後沿著 edge 到其他的 vertex，當演算法執行完時就可確保所有和起始 vertex 相連的 vertex 都有訪問到。

有 2 種常見的搜尋方式：**depth-first search** \(DFS\) 和 **breadth-first search** \(BFS\)，這兩種方式最終都可以走過所有相連的 vertex。depth-first search 用 stack 實作，而 breadth-first search 則用 queue 實作。

