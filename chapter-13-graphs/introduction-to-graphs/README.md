# Introduction to Graphs

**graph** 是類似 tree 的資料結構，而在數學上 tree 其實是 graph 的一種，但在電腦科學中 graph 和 tree 的使用情境不同。

其他的資料結構其結構通常受搭配的演算法影響，比方說 binary tree 的結構可以簡化新增和搜尋元素，而 tree 的 edge 則可以讓我們快速地從一個 node 移動到另一個 node。

但 graph 的結構通常是受實際的問題情境影響，比方 graph 的 node 可以代表城市，而連接的 edge 則代表來往城市間的航班。另一個比較抽象的例子的話，graph 表示專案中的任務，所以 node 表示這些須完成的任務，而有方向性的 edge \(指 edge 帶有箭頭以表示起迄\) 則代表任務的完成順序。在這兩個例子中，graph 的結構都來自現實的問題。

在 graph 中 node 傳統上稱為 **vertex** ，但兩者是相同的東西。

