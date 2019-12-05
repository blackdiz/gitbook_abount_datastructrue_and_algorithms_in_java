# The Efficiency of Binary Trees

如果 tree 中的每個 node 都有 2 個 child，且所有 leaf 都有相同的 level，則稱為 full tree。在 full tree 中，每個 level 的 node 數是所有在其上 level 的 node 數加 1，所以最底層的 node 數是 N / 2 + 1，N 為 node 總數，所以在尋找、插入和刪除 node 時有一半機會要到最底層的 node 才能完成。要到最底層的話，每個 level 必須 visit 1 個 node，如果 node 總數為 N，level 數為Ｌ，因為每一層的 node 都有 2 個 child，所以 level 0 有 2 個 child，level 1 有 2 ^ 2 個 child，所以有 L 層的話其有 2 ^ L 個 child 也就是有 2 ^ L 個 node，但 root 也就是 level 0 本身只有 1 個 node 故得到 N = 2 ^ L - 1，再轉換成 N + 1 = 2 ^ L，最後得到 L = log\(N + 1\)，因此在捨去常數 1 後時間複雜度為 O\(logN\)。 

如果不是 full tree 的話，我們可以說在相同 level 下，不是 full tree 的花費時間比 full tree 來得短，因為不是每個 leaf 的 level 都相同，有些會比較少，因此到底層的 時間可能更短。

但在執行許多次隨機刪除和插入後，binary search tree 會逐漸變得不平衡也就是右邊或左邊的 level 比較深，這是 binary search tree 的一個未被解決問題。

如果和其他資料結構相比，同樣有 1,000,000 個資料下，未排序陣列找到指定資料平均需要比對 500,000 次，而 tree 只需要約 20 次。

如果是已排序陣列，則尋找速度和 tree 相同，但插入和刪除資料必須移動資料位置 平均 500,00 次，tree 只需要約 20 次的比對加上一點重新連接 node 的時間 。

因此所有常見的資料操作中，tree 的效率相當好，雖然 tree 的遍歷就不像其他操作那麼有效率，但一般而言較少對大量資料做遍歷，所以影響不大。同時， tree 更適用在解析數學表示式或其他 expression 上。

