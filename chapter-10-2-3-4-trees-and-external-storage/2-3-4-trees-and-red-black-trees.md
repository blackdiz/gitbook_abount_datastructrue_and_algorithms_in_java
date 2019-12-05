# 2-3-4 Trees and Red-Black Trees

2-3-4 tree 和 red-black tree 其實是等價的，兩者可以互相轉換，2-3-4 tree 可以用下面規則轉換成 red-black tree：

* 2-node 為黑色 node
* 3-node 轉換為 parent 和 child 的組合，parent 和 child 不需要是特定的元素只要擇一即可，但 parent 須為黑色 node 而 child 為紅色 node
* 4-node 轉換為 parent 和 2 個 child 組合，用中間的元素當 parent，左右兩邊的元素分別為 left child 和 right child，相同地，parent 為黑色 node，child 為紅色 node

一旦按照這些規則轉換，2-3-4 tree 轉換後的 red-black tree 會自動符合 red-black rule。我們可以說 3-node 等於紅色 parent 和 1 個黑色 child，而 4-node 等於紅色 parent 和 2 個黑色 child，所以黑色 parent 和 1 個黑 child 不等於 3-node，而是 2 個 2-node，一個是 parent ，另一個是 child。

#### Operation Equivalence

不只是結構上二者相等，在資料操作上也是相等的，2-3-4 tree 靠 node split 保持平衡，而 red-black tree 則是用 color flip 和 rotation。

#### Efficiency of 2-3-4 Trees

#### Speed

2-3-4 tree 每一個 level 都必須 visit node，但在相同元素數目下 2-3-4 tree 比 red-black tree 的 level 來得少。2-3-4 tree 的 node 最多可以有 4 個 child，如果每個 node 都放滿元素，則 tree 的高度是 log4N，和 red-black tree 的 log2N 剛好差一半，因為元素無法全部放滿，所以高度會介於 log2\(N + 1\) 和 log2\(N + 1\) / 2 之間。

另一方面，每個 node 要比對的元素數變多會增加查詢的時間，因為需要一個個比對，所以時間耗費是線性成長，假設有 M 個元素，則需要比較 M \* log4N 次。因為 node 內元素最多為 3 個，最少 1 個，所以平均是 2，因此平均而言需要比較 2 \* log4N 次。

最後，2-3-4 tree 減少高度換得的時間和比對元素增加的時間大致抵銷，所以查詢的時間複雜度和 red-black tree 大致相同都是 O\(logN\)。

#### Storage Requirements

每個 node 都儲存了長度為 3 的陣列以記錄元素的 reference，以及長度為 4 個陣列記錄 child 的 reference。因為不一定會用到陣列裡的所有空間，如果只有 1 個元素就浪費了 2 / 3 的元素陣列空間和 1 / 2 的 child 陣列空間。所以 red-black tree 在記憶體空間的使用上比 2-3-4 tree 來得有效率。

有人可能會想說用 linked list 取代陣列，但在最多只有 4 個元素的情況下，這個差距可能很小。

在比方 Java 這種變數是存物件的 reference 而非物件本身的程式語言中，2-3-4 tree 多出來的使用空間影響不大，但在其他不是存 reference 的語言中，空間使用的效率差距可能就很顯著。



