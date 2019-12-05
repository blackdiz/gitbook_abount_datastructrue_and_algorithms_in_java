# Other Balance Trees

**AVL tree** 是另一種平衡樹，在 AVL tree 中每個 node 會記錄它的 left subtree 和 right subtree 的高度，而兩邊的差距不可以大於 1。

在插入新 node 時會檢查插入點的最低 subtree 的高度，假設差距大於 1 就用 rotation 修正，接著往上一層再檢查一次高度，一直反復檢查直到 root。

AVL tree 的查詢時間因為確保一定是平衡的，所以是 O\(logN\)，但因為在插入和刪除時需要從 root 到 leaf 找尋插入點，再從 leaf 回到 root 檢查並修正平衡，所以效率上不及 red-black tree。

另一種平衡樹是 **multiway tree** ，每個 node 可以有多個 child 如 **2-3-4 tree** 。multiway tree 的問題在於每個 node 使用的空間較大，因為需要記錄比較多 child 的 reference。

