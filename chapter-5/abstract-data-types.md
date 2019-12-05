# Abstract Data Types

**Abstract Data Types \(ADTs\)** 是指抽象的資料結構，我們只關注它們的功能而不在意它們的實作方式，stack 和 queue 就是 ADT 的例子，它們可以用 array 實作，也可以用 linked list 實作。

#### A Stack Implemented by a Linked List

之前用 array 實作 stack 時，push 和 pop 分別用:

```text
array[++top] = data // push
data = array[top--] // pop
```

而使用 linked list 時，則可以改用:

```text
list.insertFirst(data) // push
data = list.deleteFirst() // pop
```

來實作，使用 stack 的人並不知道底層是用 array 或是 linked list，在外部使用者看來，push 和 pop 的效果是相同的。

Java Code 請參照: [https://github.com/blackdiz/datastructrues\_and\_algorithms\_in\_java/blob/master/src/main/java/chapter5/LinkStack.java](https://github.com/blackdiz/datastructrues_and_algorithms_in_java/blob/master/src/main/java/chapter5/LinkStack.java)

#### A Queue Implemented by a Linked List

當使用 linked list 實作 queue 時，我們可以使用 double-ended list 的 insertLast 來作 queue.insert ，這樣每次 insert 的元素就會由前到後依 insert 先後順序排列，所以可用 deleteFirst 實作 queue.remove 達到 FIFO。

Java Code 請參照: [https://github.com/blackdiz/datastructrues\_and\_algorithms\_in\_java/blob/master/src/main/java/chapter5/LinkStack.java](https://github.com/blackdiz/datastructrues_and_algorithms_in_java/blob/master/chapter5/LinkStack.java)



