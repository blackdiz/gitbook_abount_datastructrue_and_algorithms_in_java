# Traversing the Tree

**traversing** 指以特定的順序遍歷每個 node，有 3 種簡單的方法分別是: _preorder_ 、 _inorder_ 、 _postorder_ 通常 binary search tree 使用的是 inorder。

#### Inorder Traversal

inorder traversal 表示以遞增的順序遍歷 tree 中的每個 node，如果想建立一個 tree 的排序 list，這是一個方法。

traversal 最簡單的方式是用遞迴，將 node 做為參數，每次遞迴的 method 內需要做 3 件事：

1. 用 left child 當參數，遞迴呼叫 method 遍歷左半邊的 subtree
2. visit node
3. 用 right child 當參數，遞迴呼叫 method 遍歷右半邊的 subtree

#### Java Code

```java
private void inOrder(Node localRoot) {
    if (localRoot != null) {
        inOrder(localRoot.leftChild);
        System.out.println(localRoot.iData + " ");
        inOrder(localRoot.rightChild);
    }
}
```

#### Preorder and Postorder Traversals

// TODO



