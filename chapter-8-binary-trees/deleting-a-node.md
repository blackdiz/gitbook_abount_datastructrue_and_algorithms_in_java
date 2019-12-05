# Deleting a Node

關於刪除 node，首先我們必須先找到欲刪除的 node，當找到之後，要考慮 3 種情況：

1. 該 node 是 leaf，也就是沒有任何 child
2. 該 node 有 1 個 child
3. 該 node 有 2 個 child

第 1 、 2 種情形相當簡單，但第 3 種情形就比較複雜。

#### Case 1: The Node to Be Deleted Has No Children

如果要刪除的 node 是 leaf 的話，只要將其 parent 原先指向這個 node 的 reference 改成 null 即可，node 仍然存在，但此時它就不包含在 tree 裡了。

#### Case 2: The Node to Be Deleted Has One Child

如果要刪除的 node 有 1 個 child，我們需要將其 parent 原先指向這個 node 的 reference 改指向 這個 node 的 child：

```text
    80
   /
  52
   \
   71
   /
   63
   
   // 刪除 71 的話，則將 52 和 63 連接在一起
   
    80
    /
   52
    \
    63
```

有一種特別的情況是當被刪除的 node 是 root 時，我們則將 root 下的 child 當成新的 root，而原先 child 為 root 的 subtree 就會變成新的 tree。

#### Case 3: The Node to Be Deleted Has Two Children

當被刪除的 node 有 2 個 child 時，假設我們用 right child 取代，那問題就變成要用不管是用原本 right child 下的 left child或是被刪除的 node 的 right child，都無法保持 node 在正確的順序，因此需要特別的方法處理這種清況。

在 binary search tree 中，每個 node 的次大於它的 node 稱作它的 **inorder successor**，我們先找到被刪除 node 的 inorder successor，接著用 inorder successor 放到被刪除 node 的位置，最後刪除 inorder successor。

#### Finding the Successor

找到 successor 的方式是先從該 node 的 right child 開始，接下來一路只沿著那個 right child 的 left child，中途只要遇到沒有 left child 的 node 就是 node 的 successor。

這個方法的原理在於，找尋 successor 就是在所有比被刪除的 node 大的 node 群中找最小的 node，而 binary search tree 中 right child 為 root 的 subtree 中的 node 一定比 parent 大，所以當第一步從被刪除 node 的 right child 開始時，以 right child 為 root 的 subtree 內的 node 一定都比被刪除 node 大，而我們要找尋 subtree 中最小的 node ，因為 left child 一定比 parent 小，所以接著我們一路只沿著 left child，而沒有 left child 的 node 一定是 subtree 中最小的，而如果第一步的 right child 沒有 left child，此時 right child 就是 successor。

如果 successor 有 child 會發生什麼事 ? 首先找尋 successor 時必須先到 right child 再沿著所有的 left child，**直到最後一個 left child 或者 right child 沒有 left child** ，所以找到的 successor 一定不會有 left child。另一方面，如果 successor 是**被刪除 node 的 right child**，那有 right child 則沒什麼大問題，因為當 successor 取代被刪除的 node 後，順序依然是正確的，也就是 right child 大於 parent。

所以刪除有 2 個 child 的 node 步驟是：

1. 找到被刪除的 node 的 successor
2. 將 successor 複製到被刪除 node 的位置
3. 刪除 successor

當做第 3 步驟時，因為 successor 一定沒有 child 或只有 right child，所以刪除 successor 就是套用 case 1 或 case 2 的步驟，因此我們就是將 case 3 的情況限縮成 case 1 或 case 2 來解決。

#### Is Deletion Necessary?

因為刪除的步驟相對複雜許多，所以有些人會改用別的方法，在刪除時並不會真的刪除 node，而是在 node 上標示它已被刪除 \(比方加上 isDeleted 的 flag\) ，只要標示為被刪除的 node 則不允許做任何操作，當然這樣 node 即使被刪除了依然會佔據記憶體空間，當不會做太多刪除操作時，這也是一種權宜之計。

