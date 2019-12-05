# Expanding the Heap Array

如果在程式執行過程中，heap array 滿了的話需要建立新的 heap array，將元素複製到新 heap array 中，和 hash table 不同，只需要複製而不用重新排序元素。複製的時間複雜度是 O\(N\)，但如果每次增加的長度很長比方每次增加 1 倍的話並不會經常需要擴展 heap array。

