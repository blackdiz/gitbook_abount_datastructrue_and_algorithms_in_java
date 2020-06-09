# A Tree-based Heap

除了可以用 array 實作外，也可以用 tree 實作 heap 稱為 **tree heap**。tree heap 的問題之一是找到最末端的 node，因為在移除最大 key 的 node 時需要先將最末端的 node 移動到 root 後再開始向下移動，同時也需要找到第 1 個空的 leaf node 位置 來新增 node 並向上移動，但因為它不是 binary search tree，所以我們沒辦法直接搜尋到這些目標。

但如果記錄下 node 的數量，在完整樹中要找到這些目標並不難。如同在 Chapter 8 中提到的 Huffman tree，我們可以將 root 到 leaf 的路徑用 2 進位數表示，0 為左，1 為右。

node 的數量和表示路徑的 2 進位數之間有一些關聯，假設 root 標示為 1，下一層的 node 分別為 2 和 3，第 3 層則是 4、5、6、7，以此類推。將目標 node 轉換成 2 進位數，比方在有 29 個 node 的 tree 中要找到最末端的 node，則該 node 的標示數為 29，將 29 轉成 2 進位數 11101，移除掉首位的 1 後，剩下的就是從 root 到該 node 的路徑也就是：右、右、左、右。如果目標是第 1 個空的 leaf node 也就是 30，它的 2 進位數移除首位後是 1110，所以路徑會是：右、右、右、左。

當找到目標 node 後，在向上或向下移動時 tree 的結構不會改變，所以不需要真的移動 node，可以直接將資料複製到下個位置的 node 即可，這樣就不必為了移動而重新連結 parent 和 child。此外，因為向上移動時需要 parent 的資訊，所以 node 需要有 parent 的 reference。

tree heap 的操作的時間複雜度為 $$O(logN)$$，和用 array 實作相同，多數的時間都花費在向上和向下移動的演算法中。



