# Doubly Linked List

一般的 linked list 內的 link 只指向它下一個 link，但如果需要上一個 link 時就必須由開頭到這個 link 遍歷一遍才能找到它的前一個 link，而 **Doubly Linked List** 內的 link 同時有下一個 link 的參照，也有前一個 link 的參照，因此可以直接由當下的 link 回到前一個，link 的 code 如:

```java
class Link {
  ...
  private Link next; // 指向下一個 link
  private Link previous; // 指向前一個 link
  ...
}
```

doubly linked list 的缺點是在維護 link 時比較麻煩，必須維護 4 個參照: 前一個 link 的 next、目前新 link 的 next、previous和原先位置的 previous。

#### Java Code

請參照: [https://github.com/blackdiz/datastructrues\_and\_algorithms\_in\_java/blob/master/java/chapter5/DoublyLinkedList.java](https://github.com/blackdiz/datastructrues_and_algorithms_in_java/blob/master/java/chapter5/DoublyLinkedList.java)

