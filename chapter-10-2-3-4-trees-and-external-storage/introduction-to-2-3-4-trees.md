# Introduction to 2-3-4 Trees

#### What's in a Name

2-3-4 tree 名稱中的 2、3、4 代表 node 可能有的 child 數。對不是 leaf 的 node 說，可能有 3 種結構：

* 含有 1 個元素且有 2 個 child，稱為 **2-node**
* 含有 2 個元素且有 3 個 child，稱為 **3-node**
* 含有 3 個元素且有 4 個 child，稱為 **4-node**

總結來說，非 leaf 的 node 的 child 數永遠比元素數多 1，如果 L 表示 child 的 link 數，而 D 表示元素數，則 L = D + 1。此外，node 至少都必須是含有 1 個元素和 2 個 child，所以不可能出現只有 1 個 child 的狀況。

相對的，leaf node 不會有 child 但可以含有 1 到 3 個元素，但不能是沒有包含元素的空 node。

因為 2-3-4 tree 最多可以有 4 個 child 所以又稱為 **multiway tree of order 4** 。

#### 2-3-4 Tree Organization

為了方便起見，我們將 node 中的元素按 key 遞增排列後由左至右以 0 到 2 標示，而 child link 則由左至右以 0 到 3 標示。

在 binary search tree 中，比 node 的 key 小的都會在左邊的 subtree 中，而大於或等於 node key 的都在右邊 subtree。在 2-3-4 tree 中，規則則是：

* 所有以 child 0 為 root 的 subtree 內的 node 的 key 都小於 key 0
* 所有以 child 1 為 root 的 subtree 內的 node 的 key 都大於 key 0 但小於 key 1
* 所有以 child 2 為 root 的 subtree 內的 node 的 key 都大於 key 1 但小於 key 2
* 所有以 child 3 為 root 的 subtree 內的 node 的 key 都大於 key 2

通常 2-3-4 tree 不允許重複的 key，所以不用考慮等於 key 的情況。

此外，在 2-3-4 tree 中，所有 leaf node 都會在同一 level 上。上層的 level 的 node 通常不會放滿 3 個元素。

2-3-4 tree 在插入新 node 的過程中會自我保持平衡，即使是按排序插入也一樣。

#### Searching a 2-3-4 Tree

查詢特定的元素的方法和 binary search tree 的方法相同，從 root 開始比對 key 的大小決定往哪個 child 移動直到找到目標物件或到達 null 為止。

#### Insertion

新的元素一定是放入 leaf node 中，因為如果是放在非 leaf node 裡，則依照 child 數要比元素數多 1 的規則必須修改結構以改變 child 數。

如果在找尋應該放入的 leaf node 的過程中沒有遇到已經放滿元素的 node，那麼步驟相對簡單，只要比對 key 的大小一直到 leaf node，把元素放入，再依元素間的 key 大小重新排序元素在 node 內的順序即可。

#### Node Splits

如果在過程中遇到已放滿元素的 node，那就必須做 node split 將 node 拆成 2 個，這樣 tree 才可以保持平衡。因為順序是由上往下，所以又被稱為 **top-down 2-3-4 tree** 。

我們將 node 內的元素由左到右分別命名為 A、B、C，在**非 root** 的 node 時，split 的規則為：

* 建立新的 node，位置是在被 split 的 node 右邊
* C 元素移到新 node 中
* B 元素移到 parent 內
* A 元素留在被 split 的 node
* 因為 C 元素已移到新 node 中，所以它左右兩邊的 child link，也就是最右邊的 2 個 child 改接到新 node

#### Splitting the Root

當 root 已放滿元素時，split 的步驟比較多：

* 建立新 node 並做為被 split 的 node 的 parent
* 建立新的 node，位置是在被 split 的 node 右邊
* C 元素移到新 node 中
* B 元素移到 parent 內
* A 元素留在被 split 的 node
* 因為 C 元素已移到新 node 中，所以它左右兩邊的 child link，也就是最右邊的 2 個 child 改接到新 node

在 split 後，tree 的高度就會增加 1 層但依然會保持平衡。

#### Splitting on the Way Down

因為在往下的過程中只要遇到放滿元素的 node 就會對它 split，所以不可能出現 parent 已滿而無法將被 split 的 node 內的元素移入 parent 的情形。而如果 parent 在移入元素後滿了，在下次放入新元素時就會被 split，所以也不會造成問題。

#### Java Code

請參照：[https://github.com/blackdiz/datastructrues\_and\_algorithms\_in\_java/blob/master/chapter10/tree234/Tree234.java](https://github.com/blackdiz/datastructrues_and_algorithms_in_java/blob/master/chapter10/tree234/Tree234.java)

