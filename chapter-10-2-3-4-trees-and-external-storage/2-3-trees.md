# 2-3 Trees

**2-3 tree** 是第一個發明的 multiway tree，由 J.E. Hopcroft 在 1970 發明。

2-3 tree 和 2-3-4 tree 類似，除了它能容許的元素和 child 數少 1。

#### Node Splits

node split 時，2-3 tree 和 2-3-4 tree 的步驟不同，在 2-3-4 tree 時，node 中既存的 3 個元素有 1 個留在原本的 node，1 個移到新建的 node，1 個移到 parent，然而 2-3 tree 的 node 最多只有 2 個元素，所以新加入的元素也要參與 node split。

因為新加入的元素要加入 node split，而元素又只能加在 leaf node 中，所以 2-3 tree 無法在查詢要加入的 node 的過程中就做 node split。如果要加入的 leaf node 元素未滿就可以直接加入。如果已經滿了，則將新加入的元素和 leaf node 中既存的 2 個元素排序後照 2-3-4 tree 的步驟， 1 個留在原本的 node，1 個移到新建的 node，1 個移到 parent。

如果 parent 在移入元素前就滿了也必須做 node split，相同地 parent 在 node split 後，它的 parent 也已經滿了的話一樣執行 node split，就這樣一路往上直到某次 node split 時 parent 未滿可以移入元素或者遇到 root 為止。



