# Introduction to Heaps

priority queue 是一種可以很方便存取最大或最小值的資料結構，有時會用在比方任務排程上，讓某些程式可以可比其他程式有更高的執行優先權。另一個例子是武器系統，各式各樣的威脅如戰機、飛彈等，每種威脅都有不同的優先處理順序，比方近距離的飛彈就比遠距離的戰機要優先處理。priority queue 也用在其他演算法的內部實作中，比方 graph 演算法如 Dijkstra 演算法。

priority queue 是一種抽象資料型別 \(Abstract Data Type, ADT\) 提供移除最大或最小值、新增元素等操作，如同其他 ADT，priority queue 可以用多種底層結構實作如排序 array，而排序 array 的問題在於雖然移除元素很快是 O\(1\)，但新增元素需要 O\(N\) 的時間複雜度，因為必須移動平均一半的元素才能新增元素並保持排序。

另一種實作的結構是 **heap**，heap 是一種 tree 所以新增和刪除元素的時間複雜度都是 O\(logN\)，因此雖然刪除不如排序 array 快但新增元素卻快得多，所以當涉及到許多新增元素的操作時就可以選擇用 heap。

heap 是 binary tree 且具有下列特性：

* 是完整樹，完整樹指各層的 node 全滿，除了最底層外，同時最底層的 node 全部靠左

```text
     o
   /   \
  o     o
 / \   /
o   o o
```

* 通常用 array 實作
* 每個 node 都滿足 heap condition，即每個 node 的 key 都大於或等於它的 child 的 key

因為 heap 是完整樹，所以除了最底層外的 node 都一定有 child，因此 array 中間不會有空格，每一格一定有儲存資料。



