# Double-Ended Lists

**double-ended list** 和一般的 **single-ended list** 大致相同，差別在於多了一個 last 參照到 list 內最末端的 link，這樣可以提供新增 link 到最末端的能力。當然，我們也可以遍歷到最末端後再把最末端的 link.next 指向新的 link，但相較之下這樣的做法效率不好。

可以快速新增 link 到最本端的能力使得 doubled-ended list 比 single-ended list 更適合用來實做 queue。

double-ended list 在實作 insertFirst 和 insertLast 時必須注意當 list 為空時，在新增 link 後，first 和 last 都須指向新 link 才符合結構。

而實作 deleteFirst 時，如果刪除前只有一個 link，則刪除後 first 和 last 都須指向 null 。

然而 double-ended list 因為 last 只指向最末端，但刪除最末端 link 時要前一個 link 以讓 last 可以指向新的最末端 link，因此在實作 deleteLast 時仍須遍歷整個 list 找到最末端的前一個 link。

## Java Code

請參照: [https://github.com/blackdiz/datastructrues\_and\_algorithms\_in\_java/blob/master/chapter5/DoubleEndedList.java](https://github.com/blackdiz/datastructrues_and_algorithms_in_java/blob/master/chapter5/DoubleEndedList.java)

