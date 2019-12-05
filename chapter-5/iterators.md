# Iterators

在 array 中要逐一取得元素很簡單，因為我們可以用 index 取得特定位置的元素，只要將 index + 1 就可以取到下一個元素，但在 linked list 時，沒有這樣的 index 可以使用，所以需要一個可以取得並記錄目前的 link 的參照。這個參照可以寫在 list 內部，可能叫做 **current** ，但當同時需要有多個不同位置的參照 \(例如使用 array 時就可能同時有數個變數記錄不同 index\)，我們就需要有多個不同的 current，而且沒辦法預先得知 list 的使用者會需要的數量，所以我們把這個參照獨立成另一個 class。

#### An Iterator Class

在資料結構中，含有目前指向元素的參照，同時可以用來遍歷資料結構的物件通常稱為 **iterators** ，如:

```java
class ListIterator {
  private Link current;
  ...
}
```

在設計上，通常會在 List class 中建立創造 iterator 的 method，這樣可以方便將 list 的資訊傳入 iterator 中。

#### Additional Iterator Features

list 的最前端 \(first\) 可能會改變指向的 link，比方最前端的 link 被刪除或是新增 link 在最前端，此時，iterator 要如何存取到新的 first ? 一種解決方法是將 list 的 reference 存在 iterator 中，同時 list 要提供 method 讓 iterator 可以存取 first，如: getFirst\(\) 和 setFirst\(\)，但這種方法的缺點是任何人都可以任意改變 first 指向的 link。

#### Java Code

請參照: [https://github.com/blackdiz/datastructrues\_and\_algorithms\_in\_java/tree/master/java/chapter5/iterator](https://github.com/blackdiz/datastructrues_and_algorithms_in_java/tree/master/java/chapter5/iterator)

