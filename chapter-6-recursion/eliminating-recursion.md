# Eliminating Recursion

某些計算如三角形數 \(triangle nubmers\) 和階乘 \(factorial\) 雖然也可以用遞迴，但用迴圈實作更直接，而有些如 mergesort 則是使用遞迴較直覺，但實作時，遞迴是較沒有效率的，這種時候可以藉由 stack 將遞迴轉換成非遞迴的實作。

#### Recursion and Stacks

許多 compiler 用 stack 實作遞迴的運作，當 method 被呼叫，compile 將參數和要回傳的 method 位址放入 stack 中，然後執行被呼叫的 method，等到執行完畢時，compiler 便把剛剛放入的參數、回傳 method 位址 pop 出來，接著執行回傳的 method。

#### Simulating a Recursive Method

請參照: [https://github.com/blackdiz/datastructrues\_and\_algorithms\_in\_java/blob/master/src/main/java/chapter6/stacksimulation/StackTriangleApp.java](https://github.com/blackdiz/datastructrues_and_algorithms_in_java/blob/master/src/main/java/chapter6/stacksimulation/StackTriangleApp.java)

#### What Does This Prove

 由於遞迴可和用 stack 或用迴圈彼此取代，所以一般在思考演算法的實作時可以多嚐試不同的實作看哪一種是最有效率的。

