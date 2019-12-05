# Efficiency of Heap Operations

對 heap 來說，向上和向下移動 node 是最耗時的步驟，這 2 個演算過程需要在迴圈中一直移動 node，過程中的複製次數取決於 heap 的高度，如果有 5 層則需要 4 次的複製將 node 從最上層移動到最底層。這裡我們忽略將 node 複製到暫存變數的步驟，因為這個步驟一定需要所以是常數時間。

向上移動的演算法在迴圈中只有 1 個主要步驟，用新增 node 的 key 和目前所指向位置的 node 的 key 做比較。而向下移動的演算法則需要 2 次比較，1 次比較 left child 和 right child 的 key 找出 key 較大的 child，另 1 次則是用 key 較大的 child 和被移動的 node 的 key 做比較決定是否向下移動。

heap 是 tree 的一種，而 N 個 node 的 binary tree 中 level 數 L 等於 log2\(N + 1\)，向上和向下移動的步驟需要執行 L - 1 次，所以需要 O\(logN\) 的時間複雜度。

