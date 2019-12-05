# Topological Sorting

#### 

如果我們要列出取得學位需要修完的課程可能的順序是：BAEDGCFH，依這樣的順序排序出來的 graph 稱作 **topologically sorted** ，任何必須先修的課程都排序其他的課程之前。除了上述列出的順序外，還有許多其他的順序都可以滿足修課條件，比方我們可以先修英文相關的 C 和 F 課程，這順序就會變成：CFBAEDGH，同樣可以滿足修課條件。

topological sorting 還可以適用在課程表以外的情境，一個重要的例子是工作排程，比方製造汽車時，我們需要排序好不同工作，這樣煞車才可以在裝上輪胎前先安裝上去，引擎在裝進引擎室前會先組裝完成，汽車工廠可以使用 graph 描繪出製造過程中的所有工作確保工作是依一定順序進行。

​用 graph 描繪工作排程稱為 **critical path analysis** \(關鍵路徑分析\)。此外還可以用 weighted graph 在 graph 中描述完成工作所需的時間，這樣 graph 就可以我們知道完成整個專案所需要的最短時間。

topological sorting 演算法有 2 個步驟：

1. 找出沒有 successor 的 vertex。successor 是順序上連結在 vertex 之後的 vertex，比方 A -&gt; B 的話 B 即為 A 的 successor。確認 vertex 有無 successor 的方法是在 adjacency matrix 中檢查 vertex 是否有和其他 vertex 有 edge 相連，如果沒有就表示沒有 successor
2. 將該 vertex 從 graph 中刪除後把它新增到另一個列表中。刪除 vertex 時

重覆這 2 個步驟直到 graph 中所有 vertex 都刪除後，新的 vertex 列表就是 vertex 的 topological sorting 後的順序。因為如果 vertex 沒有任何後續連結的 vertex 就表示它一定是在 topological 排序中最後一個，當它被刪除後剩下的 vertex 中一定會有一個 vertex 變成沒有後續連結的 vertex，而它就會是排序中倒數第二個，以此類推後我們可以排出 graph 中的 topological 排序。

topological sorting 可以適用在 connected graph 和 unconnected graph 上，unconnected graph 的例子是比方我們在取得數學學位的同時也想取得急救課程認證，這時兩者的修課順序沒有關連，但各自有自己的修課順序。

