# Quicksort

C.A.R. Hoare 在 1962 年發現 **quicksort**，是最流行的排序演算法，因為在大多數情況下，它是最快的排序法，時間複雜度為 $$O(N \times logN)$$ \(不過這僅限於在記憶體中排序時，如果是對硬碟上的資料排序那其他的演算也許會更好\)，基本上 quicksort 的做法是對陣列做 partition，分成大於 pivot 的一組，小於 pivot 的一組，此時可以看到陣列有一定程度的順序，接著再分別對兩組做 partition，這樣兩邊就又會呈現出順序，如此類推，直到 partition 的分組中只有一個元素不需要再做 partition 為止。

## The Quicksort Algorithm

quicksort 的簡化程式範例如下:

```java
// left 為陣列最左邊的index, right 為陣列最右邊的 index
public void recQuickSort(int left, int right) {
    if (right - left <= 0) {
        // 當 partition 的陣列只有一個元素時就視為已排序了
        return;
    } else {
        // 對 left、right 邊界定出的陣列做 partition
        int partition = partitionInt(left, right);

        // 再對 partition 左邊的分組繼續 quicksort
        // 注意 partition 時做為 pivot 的元素不須加入下次的 quicksort 中
        recQuickSort(left, partition - 1);

        // 對 partition 右邊分組繼續 quicksort
        // 同樣不將 pivot 加入下次的 quicksort
        recQuickSort(partition + 1, right);
    }
}
```

可以看出 quicksort 基本上有 3 個步驟:

1. 對陣列或子陣列做 partition，將其分成左邊資料較小的一組和右邊資料較大的一組
2. 對左邊遞迴做 partition
3. 對右邊遞迴做 partition

recQuickSort 的參數`left`、`right`定出了要做 partition 的陣列的左右邊界 ，每次做完 partition 後回傳的位置就是 pivot 所在的新位置，同時分出了下次 partition 的兩邊邊界點，而該位置本身並不會加進下次 partition 中，如:

```text
42 89 63 12 94 27 78 3 50 36
                           ^pivot
// parition 後
          left           right
           |               |
3 27 12 36 63 94 89 78 42 59
|     | ^pivot
left  right
```

## Choosing a Pivot Value

我們在挑選 pivot 時可以先嘗試下列幾點想法:

* pivot 應該從陣列中的資料挑選
* 我們可以隨機挑選一個資料當 pivot，或者為了簡化，每次都用最右邊的資料做為 pivot
* 在做完 partition 後，如果 pivot 在兩組子陣列之間，則 pivot 的新位置就是它在最終排序後的位置，因此之後的步驟都不用再移動它

關於最後一點，因為在做完 partition 後，如果 pivot 在兩組子陣列之間，pivot 左邊的元素一定都小於 pivot，不論之後左邊子陣列如何排序移動都不會影響 pivot 的位置，同理，pivot 右邊的元素一定都大於 pivot 且之後的排序也不會影響到 pivot，所以 pivot 的新位置就是它最終排序完成時的正確位置。

而如果我們找到了 pivot 應該插入的位置時該如何插入 ? 一種方法是將該位置右邊的所有元素都往右移動一格後再插入 pivot，但有更快的方法，因為右邊子陣列只是分組完確定都比 pivot 大但其內部尚未排序，所以子陣列內的元素不用保持順序，因此我們可以直接將該位置的元素和 pivot 互換，這樣 pivot 會移到應該插入的位置，而原本在該位置的元素移到最右邊也符合右邊子陣列的元素都大於 pivot 的規則:

```text
3 27 12 63 94 89 78 42 50 36
        ^                  ^
    pivot的新位置          pivot

// 將 63 和 36 互換
3 27 12 36 94 89 78 42 50 63
```

## Java Code

請參照: [https://github.com/blackdiz/datastructrues\_and\_algorithms\_in\_java/blob/master/chapter7/quicksort/QuickSort1.java](https://github.com/blackdiz/datastructrues_and_algorithms_in_java/blob/master/chapter7/quicksort/QuickSort1.java)

## Degenerates to $$O(N ^ 2)$$ Performance

pivot 最理想的情況應該是位於 partition 後的中間，這樣分組後的子陣列剛好一半小於 pivot，一半大於 pivot，這樣是最佳情況。如果子陣列有一邊的長度較長，那較長的那邊必須做更多次的 partition，因此效率就會下降。

在最糟的情形下，N 個元素被分成 1 和 N - 1 兩組子陣列，如果每次 partition 後都是這樣的結果，那每個元素就必須多一次 partition 才能分到正確的位置。以目前挑選最右邊的元素做為 pivot 的方法，在倒序的陣列中就會發生這種狀況，因為每次最右邊的元素都是當次 partition 最小的元素，所以一定會分成只有 pivot 一個元素和 N - 1 扣掉 pivot 以外其他元素兩組子陣列。

除了增加步驟而變慢外，另一個問題是因為增加步驟，所以遞迴的次數增加，而遞迴會占用 stack 的資源，如果次數過多可能就會引發 stack overflow 的問題。

因此每次挑選最右邊的元素做為 pivot 的策略在資料大致隨機分布時沒什麼問題，但當資料已正排序或倒序時就不是很好的方法。

## Median-of-Three Partitioning

我們可以遍歷整個陣列後找到中位數做為 pivot，但這樣效率不好，尋找中位數耗費的時間可能比排序還長。

一個比較實際的方法是找出陣列中最左邊、最右邊和中間這三個位置的資料的中位數做為 pivot，這個方法稱為 **median-of-three**:

```text
44 2 3 54 86 23 48 50 29
^          ^           ^
// 用 44、86、29 這三個數比較，所以中位數為 44，pivot 即為 44
```

這樣做比遍歷後找出中位數要快上許多，同時也避免用到最大或最小數做為 pivot 的問題，因為至少一定有一個數比 pivot 大，也一定有一個數比 pivot 小。

除了效率較好外，另一個好處是在演算法中:

```java
public int paritionIt(int left, int right, long pivot) {
    while (leftPtr < right && array[leftPtr] < pivot) {
    }
    while (rightPtr > left && array[rightPtr] > pivot) {
    }
    ...
}
```

`leftPtr < right` 和`rightPtr > left`的判斷式可以移除。當在比較三個數以選出 pivot 時，同時也可以對這三個數排序:

```text
44 2 3 54 86 23 48 50 29
^         ^            ^
// 44 為中位數，同時排序這三個數
29 2 3 54 44 23 48 50 86
^         ^            ^
```

當排序後最左邊的數一定大於 pivot，在 partition 時從右邊開始往中間遍歷一定會碰到大於或等於 pivot 的時候，所以`while` 迴圈不會一直往左邊執行到超出邊界。同理最右邊的數一定小於 pivot 從左邊往中間遍歷也一定會碰到小於或等於 pivot，所以不會超出右邊邊界。

此外，因為排序後，最右和最左邊的數等於已經做完 partition 分到它們正確的子陣列中，所以不用加入 partition 的陣列中，因此 partition 開始比較的位置可以設為最左邊 index + 1和最右邊 index + 1。

所以 median-of-three 不但避免使用到最大數或最小數做 pivot 造成對已排序的陣列再做排序時變成 $$O(N ^ 2)$$ 的問題，還可以減少一些判斷式和需要 partition 的資料數。

Java Code 請參照: [https://github.com/blackdiz/datastructrues\_and\_algorithms\_in\_java/blob/master/chapter7/quicksort/QuickSort2.java](https://github.com/blackdiz/datastructrues_and_algorithms_in_java/blob/master/chapter7/quicksort/QuickSort2.java)

## Handling Small Partitions

如果使用 median-of-three 方法，當子陣列的元素少於或等於 3 時就無法再用 quicksort 對其排序，3 在這裡稱作 **cutoff** ，之前的程式碼在這種情況下我們直接對 該 3 個元素比較大小做排序。

## Using an Insertion Sort for Small Partitions

另一個方法是用 insertion sort 處理小於 cutoff 的子陣列，此時 cutoff 可以設定大一點的限制如: 10、20 等，不同的 cutoff 會影響效率，Knuth 的建議是 9，但並沒有一個固定的數字，何者為最優取決於 compiler、OS、硬體等許多條件，而且因為在 quicksort 省下的比較、複製步驟因為用了 insertion sort 而增加，所以增加的效率不多。

Java Code 請參照: [https://github.com/blackdiz/datastructrues\_and\_algorithms\_in\_java/blob/master/chapter7/quicksort/QuickSort3.java](https://github.com/blackdiz/datastructrues_and_algorithms_in_java/blob/master/chapter7/quicksort/QuickSort3.java)

## Insertion Sort Following Quicksort

有些專家建議另一種方式是完全使用 quicksort，小於 cutoff 的子陣列先不處理，等到 quicksort 結束後，再做一次 insertion sort。因為 insertion sort 對於幾乎排序好做排序的效率好\(如 shellsort 的原理\)，所以這樣做理論上也可以增加 quicksort 整體的排序效率，但具體效果還是依不同環境而需要做測試。

## Removing Recursion

許多人會建議不用遞迴的方式實作 quicksort，這樣必須把每次子陣列的左右邊界記在一個 stack 中，然後再用迴圈讀出來做 partition，如此可以消除掉遞迴呼叫 method 耗費的資源。對於舊的 compiler 和電腦架構理論上這樣做會比較快，但是對於現今的系統對於處理呼叫 method 的資源耗費通常掌握得較好，因此無法肯定是否真能增加效率。

## Efficiency of Quicksort

由於 quicksort 每次遞迴時都會將子陣列分成兩半，在平均劃分的情況下 $$N$$ 個元素會劃分 $$logN$$ 次，而每次劃分完針對子陣列的 partition 的時間複雜度是 $$O(N)$$ \(請參照 [Partitioning](partitioning.md#efficiency-of-the-partition-algorithm)\)，因此 quicksort 的時間複雜度為 $$O(N \times logN)$$。

