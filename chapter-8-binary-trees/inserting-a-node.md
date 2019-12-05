# Inserting a Node

要插入新的 node 時，要先找到正確的位置，而步驟和找尋指定 key 的 node 相同，一路從 root 比對到 leaf，當比對到 leaf 時，若新 node 的 key 小於 leaf 的 key ，則新 node 為 leaf 的left child，反之則為 right child。比較特別的是，若中途有 node 的 key 和新 node 的 key 相同，則我們視為大於 node 的 key 往 right child 移動，所以我們可容許 tree 中有重複 key 的 node 存在。

#### Java Code

```java
public void insert(int id) {
    Node newNode = new Node();
    newNode.iData = id;
    
    // 當 root 為 null 時，表示 tree 中沒有任何 node
    // 因此新 node 為第一個 node 也就是 root
    if (root == null) {
        root = newNode;
    } else {
        Node current = root;
        Node parent; // 記錄目前 visit 的 node
        while (current != null) {
            parent = current;
            if (id < current.iData) {
                current = current.leftChild;
                if (current == null) {
                    // 如果 leftChild == null
                    // 則把新的 node 掛在上一層 parent 的 leftChild
                    parent.leftChild = newNode;
                }
            } else { // 大於或等於 key 都往右移動
                current = current.rightChild;
                if (current == null) {
                    // 如果 rightChild == null
                    // 則把新的 node 掛在上一層 parent 的 rightChild
                    parent.rightChild = newNode;
                }
            }
        }
    }
}
```

