# Hashing Efficiency

在沒有 collision，只呼叫 1 次 hash function 且只需要 array 的 reference 來新增或搜尋元素的情況下，新增和搜尋的時間複雜度都是 O\(1\)。

如果出現 collision，存取時間就取決於 probe length。在 probe 時每次存取儲存格時都會花費額外的時間檢查是否為空 \(當新增元素時\) 或是否是目標元素 \(當刪除或搜尋元素時\)，因此新增或搜尋的時間和 probe length 成正比，因為要加上 hash function 所耗費的常數時間。

probe length 的平均 \(影響平均存取時間\) 又取決於 load factor \(hash table 內的元素數和 hash table 容量比\)，當 load factor 增加時，probe length 會隨之增長。

#### Open Addressing

相較於 separate chaining，高 load factor 對 open addressing 的效能損失影響更大。使用 open addressing 時，失敗的搜尋花費的時間比成功搜尋要長，當 probe 時只要找到目標元素就可以停止搜尋，所以平均而言要比對 probe 次數的一半，而另一方面必須要一直搜尋到 probe 最後才能確定沒有目標元素。

#### Linear Probing

若 probe length 以 P 表示，load factor 以 L 表示，則下面等式是當成功搜尋時二者的關係：

$$
P = ( 1 + 1 / (1 - L) ^ 2) / 2
$$

而失敗搜尋的等式則是：

$$
P = (1 + 1 / (1 - L)) / 2
$$

這兩個等式引自 Knuth 的證明。

根據公式， load factor 為 1 / 2 時，成功搜尋花費 1.5 次 probe 而失敗搜尋花費 2.5 次，當 load factor 為 2 / 3 時，則成功搜尋花費 2 次 probe，失敗搜尋花費 5 次。當 load factor 超過 2 / 3 時，次數開始快速增加。

因此要把 load factor 至少保持在 2 / 3 以下，最好是保持在 1 / 2 以下。但另一方面，load factor 愈低就需要更多的記憶體空間儲存同等的元素數，因此最佳的 load factor 取決於記憶體空間和搜尋效率間的取捨。

#### Quadratic Probing and Double Hashing

quadratic probing 和 double hashing 的公式相同，當成功搜尋時為：

$$
-log2(1 - load factor) / load factor
$$

當失敗搜尋時為：

$$
1 / (1- load factor)
$$

當 load factor 在 0.5 時，成功和失敗搜尋都需要 2 次 probe，load factor 在 2 / 3 時，成功搜尋為 2.37 次，失敗搜尋為 3 次，而 load factor 為 0.8 時則分別為 2.9 次和 5 次。因此 quadratic probing 和 double hashing 相較 linear probing 可以有較高的 load factor。

#### Separate Chaining

我們想知道在 separate chaining 中搜尋或新增元素花費的時間，先假設最耗時的部分是比對 key 和 linked list 中的元素的 key，同時假定 hash 處理和決定是否已比對到 list 末端的檢查的時間花費等同於 1 次的 key 比對。因此所有的操作需要 `1 + nComps` 次，`nComps` 為比對 key 的次數。

假設 hash table 有相當於 array 容量的元素數，每個元素都儲存 1 個 list，在新增 N 個資料數時，平均而言每個 list 都會有 `N / array 長度` 個資料，所以 `list 平均長度  = N / array 長度` ，這個等式剛好和 load factor 的 `load factor = N / array 長度` 等式相同，所以 list 平均長度等於 load factor。

#### Searching

在成功搜尋到目標資料的情況下會先 hash 出 index 接著在 list 中比對元素的 key，平均而言需要比對一半的元素，因為 list 平均長度等於 load factor，所以搜尋的時間花費是 `1 + load factor / 2` 。

當搜尋失敗時，如果 list 是未排序的，因為必須比對完 list 內所有元素才能知道目標資料是不是存在，所以花費時間為 `1 + load factor`。但如果 list 已排序則平均只需要比對一半的元素就可以確定是否存在目標資料，所以花費的時間和成功搜尋時相同。 

通常在 separate chaining 中會讓 load factor 保持在 1 左右，也就是資料數等同於 array 長度，更小的 load factor 並不會增加太多效率，但所有操作的時間花費會隨著 load factor 變大而線性成長，所以通常不會讓 load factor 大於 2。

#### Insertion

如果 list 不需排序，新增元素就不用比對 key 大小找到正確的位置而可以直接新增在 list 末端，但因為仍要進行 hash，所以新增的時間為 1。

而如果 list 是排序的，因為失敗搜尋必須比對一半的元素才能找到資料新增的位置，所以新增時間為 `1 + load factor / 2` 。

#### Open Addressing Versus Separate  Chaining

如果使用 open addressing，那 double hashing 是最好的方式，除非是在有很多記憶體可以使用且在建立好 hash table 後不需要擴展時，這種情況下 linear probing 比較容易實作，而且如果可以保持 load factor 在 0.5 以下，那效率的損失也不大。

但假如無法確定會新增多少資料，那 separate chaining 是比較好的選擇，因為 load factor 變大對 separate chaining 的效能損失是線性的比 open addressing 來得小。

所以當不確定應該採用何種方式時，先使用 separate chaining，雖然實作時需要額外實作 linked list，但當新增愈來愈多資料時不會造成效能損失。



