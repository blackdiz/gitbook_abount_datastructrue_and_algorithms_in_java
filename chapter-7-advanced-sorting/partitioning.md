# Partitioning

劃分\(**partition**\)表示將資料分成兩組，高於某個數值的歸為同一組，另一組則都是低於該數值的，而用於劃分資料的數值稱稱為 **pivot** 。在劃分資料後，資料並沒有被排序，只是單純分成兩組，但比原始狀態更有序一些，同時每組資料的順序也不會和原始狀態相同。

我們可以分別在最左和最右邊用一個指標標示目前取到的元素位置，然後開始往中間遍歷，當左邊指標取到的元素大於 pivot，而右邊指標元素取到的元素小於 pivot 時，就將兩者資料交換，接著繼續往中間遍歷直到兩個指標交會時表示整個陣列的資料都已經和 pivot 比較過可以結束劃分。

#### Java Code

請參照: [https://github.com/blackdiz/datastructrues\_and\_algorithms\_in\_java/blob/master/src/main/java/chapter7/partition/Partition.java](https://github.com/blackdiz/datastructrues_and_algorithms_in_java/blob/master/src/main/java/chapter7/partition/Partition.java)

#### Efficiency of the Partition Algorithm

劃分時，最左和最右邊的指標會往中間遍歷取出元素和 pivot 比較，直到兩者交會剛好遍歷完所有元素一次，所以比較次數為 N 次，時間複雜度為 O\(N\)，不論資料如何分佈都不影響比較的時間複雜度。

而當指標取出的元素和 pivot 比較後發現位置不對時需要互換位置，而互換的次數就取決於資料如何分佈，如果資料剛好是倒序排序，則分成一半時兩邊的每個元素都需要兩兩互換，因此有 N 個元素時，交換次數會是 N / 2 次。

如果是隨機分佈的話，而 pivot 剛好將資料對半劃分時，那次數會少於 N / 2，因為一定有些資料一開始就已經在正確的位置上。如果 pivot 劃分時是偏向大於 pivot 或小於 pivot 的資料較多這種偏斜的劃分時，那互換次數會更少，因為某一邊的資料量少表示互換次數也會隨之減少，比方假設大於 pivot 的只有一個元素，那表示只有兩個元素擺錯位置，因此互換次數只有一次。

由於互換次數和 N 也是成正比關係，因此 partition 的總體時間複雜度為 O\(N\)。

