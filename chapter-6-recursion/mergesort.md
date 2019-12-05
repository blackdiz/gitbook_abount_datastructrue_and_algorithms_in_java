# mergesort

**merge sort** 在速度上比 bubble sort、insertion sort 和 selection sort 快上許多，後三者的時間複雜度為 O\(N^2\)，而 merge sort 的時間複雜度是 O\(N \* logN\)，但它的缺點是需要額外和欲排序的元素總量大小相等的記憶體空間。

#### Merging Two Sorted Arrays

merge sort 的作法是合併兩個已排序的陣列 A、B 產生新的已排序陣列 C，假設 A 陣列有 23、47、81、95 四個元素，B陣列有 7、14、39、55、62、74 六個元素，則排序時每次取 A、B 陣列中最小數字相比，較小的就放入 C 陣列中，然後重複直到所有數字比完或其中一個陣列已比過所有元素時就不需再進行相比直接把另一個陣列中剩餘尚未比過的元素直接放入 C 陣列中，以表格表示的話:

| 步驟 | 比較 | 複製 |
| :--- | :--- | :--- |
| 1 | A: 23, B: 7 | 將 7 從 B 放入 C |
| 2 | A: 23, B: 14 | 將 14 從 B 放入 C |
| 3 | A: 23, B: 39 | 將 23 從 A 放入 C |
| 4 | A: 47, B: 39 | 將 39 從 B 放入 C |
| 5 | A: 47, B: 55 | 將 47 從 A 放入 C |
| 6 | A: 81, B: 55 | 將 55 從 B 放入 C |
| 7 | A: 81, B: 62 | 將 62 從 B 放入 C |
| 8 | A: 81, B: 74 | 將 74 從 B 放入 C |
| 9 | B 已無元素，不用比較 | 將 81 從 A 放入 C |
| 10 | B 已無元素，不用比較 | 將 95 從 A 放入 C |

#### Sorting by Merging

merge sort 的概念是將陣列分成兩半，分別排序後再合併，那如何排序那兩半呢? 就是把分割後的陣列再分成兩半，排序後再合併回來，以此類推會一直分割，直到只剩一個元素就不需排序後再合併回去。

在遞迴版本的 merge sort 中，我們不是從既存的兩個陣列合併成一個，而是從要排序的陣列一直分割到不用排序為止，再合併排序回原本的陣列中。

#### Java Code

請參照: [https://github.com/blackdiz/datastructrues\_and\_algorithms\_in\_java/blob/master/java/chapter6/mergesort/MergeSort.java](https://github.com/blackdiz/datastructrues_and_algorithms_in_java/blob/master/java/chapter6/mergesort/MergeSort.java)

#### Efficiency of the mergesort

mergesort 的時間複雜度來自分割陣列的次數和比較的次數。

從分割陣列的次數來看，由於每次都是將陣列分成兩半，也就是 / 2，直到無法在分割為止，所以反過來看就是 2 ^ X = 陣列的長度 N，所以 X = logN，所以分割的時間複雜度是 O\(logN\)。

從比較次數來看，每次兩個陣列 A、B 長度 M、N 相比時，最糟的情況下兩個陣列的元素會交錯比較到，直到最後一個元素不用比較，所以會是 M + N - 1，如:

```text
A: 1 3 5 7 9
B: 2 4 6 8 10
比較步驟
     A   B
步驟1 1 < 2
步驟2 3 > 2
步驟3 3 < 4
步驟4 5 > 4
步驟5 5 < 6
步驟6 7 > 6
步驟7 7 < 8
步驟8 9 > 8
步驟9 9 < 10
```

而在最佳情況下，若 A 陣列的元素均比 B 陣列小，則只須比較 A 陣列的長度 M 次數即可，如:

```text
A: 1 2 3 4
B: 5 6 7 8
比較步驟
     A   B
步驟1 1 < 5
步驟2 2 < 5
步驟3 3 < 5
步驟4 4 < 5
```

若原始長度恰好可以 / 2，那 A、B 陣列的長度都相同為 N，所以最糟情況下比較次數為 2N -1，最佳情況下是 N，可以說比較的時間複雜度是 O\(N\)。

所以 mergesort 的步驟為需要分割 logN 次，每次分割後要合併時須比較 N 次，故總共要比較 N \* logN 次，故時間複雜度為 O\(N \* logN\)。

