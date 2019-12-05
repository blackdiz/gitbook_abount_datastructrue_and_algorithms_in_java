# Rotations

要平衡 tree 必須重新排列 node 的位置，比方如果 node 集中在左邊就必須把一部分移到右邊，我們透過 **rotation** \(旋轉\) 來達到這個目的。

在做 rotation 時必須同時做到：

* 改變 node 的位置以保持 tree 的平衡
* 確保 binary search tree 的特性，也就是每個 node 的 left child 小於它，而 right child 大於或等於它

#### What's Rotating

rotation 並不是真的旋轉 node 而是改變 node 間的關係，當有 1 個 node 被選為 rotation 的 top node 後，如果往右旋轉，則 top node 會移到原先它的 right child 的位置，而它的 left child 則會移到它本來的位置：

```text
  1
 /\
2  3

// 往右旋轉後
 2
  \
   1
    \
     3
```

#### Mind the Children

因為 rotation 後，和 rotation 反方向的 child 會移到 top node 的位置，所以 top node 一定要有對應的 child node，比方向右旋轉時要有 left child，而向左旋轉時要有 right child。

#### The weird Crossover Node

```text
     50
     /\
   25  75
   /\
 12  37
 
 // 以 50 為 top node 向右旋轉後
 
      25
      /\
    12  50
        /\
      37  75
```

可以發現 37 從 25 的 right child 變成 50 的 left child，它不是向下或向上移動而跨過原先的 node 連結到別的 node。

在 rotation 前，37 稱為 50 的 **inside grandchild**，而 12 為 50 的 **outside grandchild**，而 50 則是它們的 **grandparent**。如果 inside grandchild 的 parent 是 rotation 時會往上移動的 node \(也就是往右旋轉時 top node 的 left child 或往左旋轉待的 right child\)，它會切斷和原先 parent 的連結而重新連結到它的 grandparent。

