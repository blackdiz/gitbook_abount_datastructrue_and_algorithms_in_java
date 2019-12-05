# Sorted Lists

顧名思義，sorted list 中的元素都經過排序，而刪除通常只允許刪除最大或最小的元素。在多數情形下，可以用 sorted list 取代 sorted array，而 sorted list 的優勢在於新增時較快，因為它不需要移動元素的位置，此外 sorted list 較不浪費記憶體，是隨著元素數量增加而增加使用的記憶體。

#### Java Code

請參照: [https://github.com/blackdiz/datastructrues\_and\_algorithms\_in\_java/blob/master/java/chapter5/SortedList.java](https://github.com/blackdiz/datastructrues_and_algorithms_in_java/blob/master/chapter5/SortedList.java)

#### Efficiency of Sorted Linked Lists

在已有 N 個元素的 sorted list 內新增、刪除在任意位置的元素需要遍歷 list，平均需要 N / 2，最糟的狀況是所有元素都比對過一次，須比對 N 次，所以時間複雜度為 O\(N\)。而查詢、刪除最小值\(或最大值，端看如何排序\)，因為都是直接存取最前端的元素，因為複雜度為 O\(1\)。

如果是在一個空的 sorted list 中開始新增元素，每次新增元素時都需要和之前放入 sorted list 內的元素逐一比對，最壞的狀況下每次都須從頭比對到尾，所以有 N 個元素時，須比對 N - 1 次，因此時間複雜度為 O\(N \* \(N - 1\)\) = O\(N^2 - N\) = O\(N^2\)。

如果使用情境是經常存取最小或最大值且不要求快速新增的話，sorted list是不錯的選擇。

#### List Insertion Sort

sorted list 可以用來做為頗有效率的排序機制，假設現在有一個未排序的 array，我們可以把 array 中的元素一個一個取出放入 sorted list 中，最後 sorted list 內就會是已排序的狀態，接著再從 sorted list 最前端一個一個取出放回 array 中就可以得到一個已排序的 array。

如上所述，雖然比對的時間複雜度和 insertion sort 相同為 O\(N^2\)，但每個元素只須要複製兩次，所以複製的時間複雜度為 O\(2N\)，相較 insertion sort 的 O\(N^2\) 要好。

但這種排序法的缺點在於需要比 insertion sort、bubble sort、selection sort 多一倍的記憶體空間。

#### Java Code

請參照: [https://github.com/blackdiz/datastructrues\_and\_algorithms\_in\_java/blob/master/java/chapter5/ListInsertionSortApp.java](https://github.com/blackdiz/datastructrues_and_algorithms_in_java/blob/master/chapter5/ListInsertionSortApp.java)

