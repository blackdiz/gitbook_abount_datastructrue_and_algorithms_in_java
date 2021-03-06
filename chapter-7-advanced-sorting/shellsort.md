# Shellsort

**Shellsort** 以其發明人 Donald L. Shell 命名，是以 insertion sort 為基礎但加入一些新的特性而大幅提升效率。

shellsort 對中等長度的陣列大約幾千個元素以下的排序效能不錯，雖然不比 quicksort 或其他 O\(N \* logN\) 的排序法來得快，但相較 O\(N ^ 2\) 好很多。由於它在最糟情況下的效率和平均情況時相差不多， 而且實作容易，有些人建議先用 shellsort 實作， 遇到不符效能要求的情況再換成其他更快的實作。

## Insertion Sort: Too Many Copies

insertion sort 的做法是假設陣列左邊是已排序，右邊都未排序，接著我們選定一個未排序的元素，比較已排序陣列中的元素，當找到要插入的位置時，先把該位置以右的元素都往右移動一位，在將元素插入。

insertion sort 的問題在於，假設最小的元素一開始在最右邊，當它要插入到陣列最左時必須移動中間的所有元素也就是整個陣列的元素，因此需要 N 次複製。平均情況下每個元素需要複製 N / 2 次，所以總共是 N \* N / 2 次，時間複雜度為 O\(N ^ 2\)。

## N-Sorting

shellsort 的作法是先對大間隔的元素做 insertion sort，排序完後縮小間隔再排序一次，這些間隔稱為 **increment** ，一般用 **h** 表示。以一個長度為10的陣列用 increment = 4 做 shellsort 為例，一開始先對第 0、4、8 個元素排序，之後往右移動一格再對 1、5、9 排序，一直到整個陣列都以 4 為一個單位排序過，也就是每個相隔 4 個間隔的元素彼此都是已排序，如下例:

```text
7 10 1 9 2 5 8 6 4 3 // 先對第 0、4、8 個元素排序
^        ^       ^

2 10 1 9 4 5 8 6 7 3 // 再對第 1、5、9 個元素排序
  ^        ^       ^

2 3 1 9 4 5 8 6 7 10 // 對第 2、6 個元素排序
    ^       ^

2 3 1 9 4 5 8 6 7 10 // 對第 3、7 個元素排序
      ^       ^

2 3 1 6 4 5 8 9 7 10 // 因第 4 個元素在第一步已排過，所以結束
```

此時我們可以將陣列看成有 4 組小陣列: \(0, 4, 8\)、\(1, 5, 9\)、\(2, 6\)、\(3, 7\)，每一個小陣列本身都是排序過的，它們在原始陣列中彼此交錯但不相關聯。同時在這個例子中可以觀察到每個元素距離它應該被排序的位置最多不會超過 2 格，所以原始陣列此時已經接近完全排序的狀態，這就是 shellsort 的改進之處，經由預先排序出交錯的小組陣列來減少每個元素的移動次數。

由於 insertion sort 在對幾乎已排序的陣列進行排序時十分有效率，如果每個元素只需要移動 1 ~ 2 格就可以到正確的位置，那時間複雜度可以接近 O\(N\)，所以在做完 4 組小陣列的排序後，最後我們只要再進行一次間隔為 1 的普通 insertion sort 就可以了，這樣組合不同間隔做 insertion sort 的做法比單純直接 insertion sort 來得快很多。

## Diminishing Gaps

陣列的長度越長，起始間隔就越大，然後一直縮減直到等於 1 為止，比方長度 1000 的陣列，起始間隔可能為 364，接下來是 121，接著是 40，然後 13 ，4，最後為1，這個間隔的數列稱作 **interval sequence** 或 **gap sequence** ，前述這個數列的變化來自 Knuth 的公式:

$$
h = 3 * h + 1
$$

用表格列出來就是:

| h | 3 \* h + 1 | \(h - 1\) / 3 |
| :--- | :--- | :--- |
| 1 | 4 |  |
| 4 | 13 | 1 |
| 13 | 40 | 4 |
| 40 | 121 | 40 |
| 121 | 364 | 121 |
| 1093 | 3280 | 364 |

由於 1093 大於陣列長度 1000，所以我們就從 364 開始，然後每次排序完後再按公式的反轉公式:

$$
h = (h - 1) / 3
$$

算出下一個間隔。

## Java Code

請參照: [https://github.com/blackdiz/datastructrues\_and\_algorithms\_in\_java/blob/master/chapter7/shellsort/Shellsort.java](https://github.com/blackdiz/datastructrues_and_algorithms_in_java/blob/master/chapter7/shellsort/Shellsort.java)

## Efficiency of the Shellsort

目前還沒能分析出 shellsort 確切的效率理論值，經過實驗有數種結果，從O\(N ^ 3/2\) 到 O\(N ^ 7/6\)都有。

