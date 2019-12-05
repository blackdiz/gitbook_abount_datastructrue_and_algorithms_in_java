# Balanced and Unbalanced Trees

binary search tree 有一個問題是如果插入的 key 是排序過的，那會形成不平衡的 tree，假如是正序的話，所有 node 都會變成 right child，倒序則變成 left child，不論哪種都會讓 tree 的 node 集中在某一邊。

#### Degenerates to O\(N\)

當 tree 的 node 都是 parent 的 right child 或都是 left child 時，實際上就變成 linked list，如：

```text
// 都是 right child 的狀況
10
 \
  20
   \
   30
    \
    40
     \
      50
```

所以尋找指定資料的時間複雜度會從 O\(logN\) 變成 linked list 的 O\(N\)。

而部分已排序同樣會造成 tree 有某部分不平衡，雖然不像上述情況那麼嚴重，不過依然沒辦法發揮出 binary search tree 的最大效率，而時間複雜度會介於 O\(N\) 至 O\(logN\) 之間。

#### Balance to the Rescue

為了保證 O\(logN\) 的速度，我們必須確保 tree 至少接近完全平衡，所以每個 node 的左右兩邊下面 node 數必須接近相等。

在 red-black tree 中，插入元素時會檢查是否保持平衡，如果沒有的話會重新排列 tree 的結構以確保 tree 是平衡的。

#### Red-Black tree Characteristics

red-black tree 有下列 2 個特性：

1. 所有 node 都以顏色標記
2. 在插入和刪除 node 時須保接這些顏色按特定規則排列

#### Colored Nodes

red-black tree 中顧名思義，每個 node 不是標為紅色就是標為黑色，實際上紅黑只是用來分類而已，也可以用其他顏色或分類代替。

#### Red-Black Rules

當插入和刪除 node 時要按下面規則操作，稱作 **red-black rule** ：

1. 所有 node 非紅即黑
2. root 永遠是黑色 node
3. 如果 node 是紅色，則 child 必須是黑色，換句話說任何相鄰的 node 不可同時為紅色 \(相鄰指的是有 link 連結的 node，所以是 parent 和 child 的關係\)
4. 每條從 root 到 leaf node 或是 null child 的 path 上必須有相同數量的黑色 node

第 4 點中的 **null child** 指的是接到非 leaf 的 node 上的 child，比方當 node 只有 right child 時，我們用一個資料為 null 的 node 接到它的 left child 上。

而從 root 到 leaf node 的 path 上的黑色 node 數稱為 **black height** ，所以第 4 點換個方式說就是所有 root 到 leaf 的 path 的 black height 都相同。

#### The Red-Black Rules and Balanced Trees

想要建立一個差距 2 個 level 以上的不平衡 red-black tree 是不可能的，如果某條 path 比另一條多於 1 個 node，那一定是含有較多的黑色 node 因此違反第 4 點規則，不然就是有 2 個相鄰的紅色 node 而違反了第 3 點規則。





