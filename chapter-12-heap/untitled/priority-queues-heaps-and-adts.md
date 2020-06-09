# Weakly Ordered

相較 binary search tree 中 1 個 node 的左邊以下的 node 的 key 一定小於右邊以下的 node，heap 的順序性較弱。所以我們用簡單的演算法就可以按 node 的順序遍歷 binary search tree。

但在 heap 中，要按 node 順序遍歷很難，因為它的組織原則 \(也就是 heap condition\) 不像 binary search tree 那麼強，我們只確定每一條從 root 到 leaf 的路徑上的 node 都是降序排序，但任一 node 左右兩邊以下的 node 或不在和它同一路徑上的上下層 node 間沒有順序關係。

因為 heap 是弱順序性，有些操作很困難甚至是不可能的，例如除了遍歷外 heap 沒辦法快速地在 $$O(logN)$$ 內搜尋指定 key 的 node，因為我們沒辦法確定搜尋時要往 node 的 left child 還是 right child 移動，同時這也表示沒辦法快速的刪除指定的 node。雖然可以遍歷 array 的元素來搜尋或刪除指定的 node，但這樣時間複雜度會是 $$O(N)$$。

但弱順序性下還是可以快速刪除最大元素和新增元素，而實作 priority queue 也只需要這些操作就足夠。

