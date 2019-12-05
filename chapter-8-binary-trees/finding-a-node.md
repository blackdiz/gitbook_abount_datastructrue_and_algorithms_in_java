# Finding a Node

尋找指定的 key 在所有操作是最容易實作的，因為 binary search tree 中 left child 一定大於 parent 且 right child 必大於 parent，所以我們從 root 開始比較 node 中的 key 和指定的 key 如果 key 比 node 小，那可能的 node 就一定在左邊，所以就用 root 的 left child 做為下一個比較的 node；反之如果 key 比 node 大，那就以 root 的 right child 當下個比較的 node，以此反覆進行直到找到相同 key 的 node，如果到達 leaf 還是沒有找到 node 則表示 tree 中沒有含有指定 key 的 node。

#### Java Code

```java
public Node find(int key) {
    Node current = root;
    
    while (current.iData != key) {
        if (current.iData < key) {
            // 當 key 比較小時，往 left child 移動
            current = current.leftChild;
        } else {
            // 當 key 比較大時，往 right child 移動
            current = current.rightChild;
        }
         
        // 當 child 為 null 時，表示已經到 leaf 了
        // 所以沒找到相同 key 的 node
        if (current == null) {
            return null;
        }
    }
    
    return current;
}
```

