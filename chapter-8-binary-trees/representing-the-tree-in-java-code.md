# Representing the Tree in Java Code

如同其他的資料結構，tree 也有多種實作方法，最常見的方法是在記憶體任意處儲存 tree 的 node，然後在 node 中用 reference 指向它 child 的位置。另一種方法是用陣列儲存，每個 node 在 tree 的位置對應到它在陣列中的位置。

以下先以 reference 指向的方法實作。

#### The Node Class

```java
class Node {
    int iData;
    Node leftChild;
    Node rightChild;
    ...
}
```

node class 中以 `leftChild` 和 `rightChild` 分別指向其下兩邊的 child。有些實作會加上指向 parent 的 reference，這樣在實作某些 tree 的操作會比較簡單，但也會讓另一部分的操作實作變複雜，因此這裡並不加上 parent。

#### The Tree Class

代表 tree 的 class 中只有一個指向 root 的 reference，其他 node 都是經由 root 來 visit :

```java
class Tree {
    private Node root;
    ...
}
```



