# Separate Chaining

**separate chaining** 是在 array 中放 linked list，當有 collision 時直接新增在該 index 的 linked list 末端。

#### Load Factors

**load factor** 指 hash table 的元素數和容量的比例，使用 separate chaining 的話，因為每個 array 的格子裡的 list 可以新增 N 個元素，所以 load factor 可以大於 1。

如果 list 內有太多元素，則搜尋時間就會增加，因為在 linked list 中平均需要檢查 N / 2 個元素才可以找到目標元素，所以我們不希望 list 太滿。

以 load factor 等於 1 的情況，大約有 1 / 3 的格子是空的，1 / 3 的格子的 list 只有 1 個元素，1 / 3 的格子的 list 有 2 個以上的元素。

使用 open addressing 時，當 load factor 達到 1 / 2 或 2 / 3 左右時，效率就會下降。但 separate chaining 即使在 load factor 大於 1 也不會影響效率太多，所以當很難預測會放入多少資料時，separate chaining 是比較穩健的機制。

#### Table Size

不像 double hashing，separate 因為沒有 probe，所以 array 長度並不要求必須是質數，但另一方面，當 array 長度不是質數時，某些 key 的分布情況會導致 cluster 出現。

#### Buckets

Data Structures and Algorithms in Java 的定義：

另一種類似 separate chaining 的方式是不用 linked list 而改用 array，這些 array 通常稱為 **bucket**。這種方法不比用 linked list 來得有效率，因為很難決定 bucket 的長度，如果太小則很快就裝不下資料，如果太大會浪費記憶體空間。

wiki 的定義：

array 中每一格即稱為 bucket，open adresing 的 bucket 儲存 key 和資料，而 separate chaining 儲存 linked list  的 reference。

#### Java Code

請參照：[https://github.com/blackdiz/datastructrues\_and\_algorithms\_in\_java/blob/master/src/main/java/chapter11/hashchain/HashChainHashTable.java](https://github.com/blackdiz/datastructrues_and_algorithms_in_java/blob/master/src/main/java/chapter11/hashchain/HashChainHashTable.java)

