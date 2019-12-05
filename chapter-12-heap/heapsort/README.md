# Heapsort

heap 的高效率可以用來實作一種簡單且效率很好的排序演算法稱作 **heapsort** 。

heapsort 的基本概念是將未排序的元素新增到 heap 中，接著再一一從 heap 中取出，這樣取出時就會依排序的順序取得元素。

因為新增和刪除的時間複雜度均為 O\(logN\)，當有 N 個元素時整個 heapsort 的時間複雜度即為 O\(N \* logN\) 和 quicksort 相同。不過相較於 quicksort，heapsort 會較慢，因為在向下移動的步驟中需要較多操作，然而有一些方法可以增加 heapsort 的效率。

