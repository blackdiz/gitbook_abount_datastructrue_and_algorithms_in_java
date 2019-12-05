# Finding Maximum and Minimum Values

#### Minimum Value

因為 binary search tree 的 left child 一定小於 parent，所以只要一直沿著 left child 直到最後一個 left child 就會是最小值。

#### Java Code

```java
public Node minimum() {
    Node current, last;
    current = root;

    while (current != null) {
        last = current;
        current = current.leftChild;
    }
    
    return last;
}
```

#### Maximum Value

同理，right child 一定大於 parent，所以沿著 right child 直到最後一個 right child 會是最大值。

#### Java Code

```java
public Node maximum() {
    Node current, last;
    current = root;
    
    while (current != null) {
        last = current;
        current = current.rightChild;
    }
    
    return last;
}
```

