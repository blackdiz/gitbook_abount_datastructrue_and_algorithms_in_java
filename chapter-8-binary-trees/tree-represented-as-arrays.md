# Tree Represented as Arrays

tree 也可以用陣列的方式實作，陣列中的位置表示該 node 在 tree 中的位置，root 的 index 為 0，而 root 的 left child 為 1，right child 為 2，以此類推，如果 node 不存在則插入 null 或 0 表示。

因此每個 node 的 left child 的 index = node index \* 2 + 1，right child = node index \* 2 + 2，而 parent = \(node index - 1\) / 2 的商數。

在大多數情況下用陣列實作不是很好的選擇，不存在或已被刪除的 node 仍然會浪費記憶體，而且如果被刪除的 node 下有自己的 subtree 還必須花費時間將它們移動到正確位置。然而假如不允許刪除 node，則比方有些環境下動態分配記憶體相當耗時的話則可以考慮使用陣列實作。

