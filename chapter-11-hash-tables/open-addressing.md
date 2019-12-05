# Open Addressing

#### Open Addressing

當用 open addressing 時，如果資料 hash 後的 index 無法存入資料，我們就在 array 中搜尋另一個位置儲存，搜尋的方法可以用 **linear probing** 、**quadratic probing** 、**double hashing** 。

#### Linear Probing

linear probing 指線性地往後搜尋直到找到空格可以存入，比方 5,421 已被使用就往後找 5,422，接著 5,423 以此類推遞增 index 直到找到空格。 

在過程中你會發現有存入資料的格子在 array 中的分布不平均，有時候會連續出現數個空格，有時候則連續出現好幾個有存入資料的格子，那些連續著存入資料的格子稱為 **filled sequence** ，當愈來愈多資料存入，filled sequence 會變得更長，這種狀況稱作 **clustering** ，同時存入資料也會變得愈來愈慢。

當我們要搜尋資料時，輸入 1 個 key 將它 hash 後取得 index 再到 array 該格讀取資料，讀出來的貧料可能就是我們想要的，但也有可能因為 collision 所以目標資料是存放在其他格中，這時候就要往下一格比對，這個過程稱作 **probe** 。

因為在存入資料時，collision 的資料是往後循著格子找到第 1 個空格放入，所以 collision 的資料一定是連續儲存在 array 中，因此在 probe 時如果遇到空格就表示 array 中沒有該筆資料。

在刪除時我們不能直接將資料移除就好，因為在搜尋步驟中一旦遇到空格就視為查無資料，所以如果只將資料移除就會在中間產生空格而使搜尋操作不正確。我們可以在移除資料後用特別的標示資料存入此格，這樣在新增、搜尋資料時只要遇到此標記的資料就繼續往下格移動，但如果刪除大量資料後，hash table 內會被這些無用的標記資料占據大量空間使得操作效率愈來愈低，因此許多 hash table 的實作都不允許刪除資料。

Java code 請參照[https://github.com/blackdiz/datastructrues\_and\_algorithms\_in\_java/blob/master/src/main/java/chapter11/linearprobe/LinearProbeHashTable.java](https://github.com/blackdiz/datastructrues_and_algorithms_in_java/blob/master/src/main/java/chapter11/linearprobe/LinearProbeHashTable.java)

#### Clustering

當新增或搜尋資料時，要找到 collision 後存在其他格的正確資料所移動的格數稱作 **probe length** ，而當 hash table 愈來愈滿時， cluster 的部分也會愈來愈大，clustering 會造成很長的 probe length，所以要存取末端的儲存格也會變慢。

當 array 存放的資料愈多時，clustering 就愈嚴重，通常當 array 為 1 / 2 滿到 2 / 3 滿時問題不會太嚴重，一旦超過這個界限效率就會嚴重下滑。所以設計 hash table 時要確保內容永遠不會存放超過 1 / 2 最多 2 / 3 的容量。

#### Expanding the Array

當 hash table 的容量快滿時可以擴展 array 的長度，因為在 Java 中 array 的長度是建立時就固定了，所以必須新建 1 個較長的 array 再將原本 array 中的資料複製到新 array 中。

因為 hash 中 hash 值是 `% array size` 所以不能直接 1 對 1 地將舊  array 的資料放到新 array 中的相同 index，必須把每個資料的 key 用新 array 的長度重新 hash 得到在新 array 中的 index，這個步驟稱為 **rehashing** ，是很耗時的過程但如果想擴展 array 的長度就無法避免。

新 array 的長度通常是舊 array 的 2 倍，而由於 hash table 的 array 長度最好為質數，所以會稍稍大於 2 倍。我們可以用下面試除法的程式找出第 1 個大於 N 的質數：

```java
// 找出第 1 個大於 min 的質數private int getPrime(int min) {    for (int i = min + 1; true; i++) {        if (isPrime(i)) {            return i;        }    }}// 任何合數 n = a * b，a、b 為質數，則可能的組合為 1 < a <= b < n// 因為 a <= b 所以如果分別 * a 和 * b // 可得 a ^ 2 <= a * b 和 a * b <= b ^ 2// 故 a ^ 2 <= n <= b ^ 2// 再開方根可得// a <= sqrt(n) <= b// 故 n 的因數最大為 sqrt(n)// 如果 2 ~ sqrt(n) 中間有可以整除 n，則 n 即為合數，反之則為質數private boolean isPrime(int n) {    for (int i = 2; i * i <= n; i++) {        if (n % i == 0) {            return false;        }    }    return true;}
```

#### Quadratic Probing

linear probe 當出現 clustering 時，cluster 的部分會持續增長，而 cluster 的部分愈長，hash 後的 index 就愈有可能落在 cluster 的區間中而讓資料新增在 cluster 的末端進而讓 cluster 又變更長形成惡性循環。

hash table 的資料量和 hash table 的容量大小比值稱為 **load factor** ，即 `load factor = 資料數 / array 長度` 。

即使 load factor 不高還是可能出現 cluster，原因就在於上述的 cluster 成長循環。

**quadratic probing** 是一種試圖防止出現 cluster 的方法，它原理是當 probe 時找尋較分散的其他空格而不是找尋最鄰近的空格。

#### The Step Is the Square of the Step Number

在 linear probe 中，如果 index x 有 collision，則 probe 時是 x + 1，x + 2，x + 3 遞增，而在 quadratic probe 時則是取步驟數的平方 x + 1 ^ 2，x + 2 ^ 2，x + 3 ^ 2 如此增加，因此讓新增時的空格較分散而較不容易出現 cluster。

#### The Problem with Quadratic Probes

quadratic probe 防止了 linear probe 會產生的 clustering 問題 \(又稱 **primary clustering**\)，但會有另一種 clustering，當 key 的 hash index 相同時 probe 的步距是相同的，所以  probe length 會愈來愈長，這種情況稱作 **secondary clustering** 。

#### Double Hashing

因為 secondary clustering 的原因是產生 probe 的步距數列一律是 1、4、9...，所以需要一個方法產生的步距數列是依賴 key 不同而相異的。

這個方法就是用不同的 hash function 對 key 再 hash  一次，而 hash 後的結果就做為這個 key 的步距，所以對於相同的 key 步距一定相同，但不同的 key 就會相異。

依照經驗，產生步距的 hash function 必須有下列特點：

* 必須和產生 index 用的 hash function 不同
* 結果不可以是 0，不然就無法前進

而有人已經發現 `步距 = constant - (key % constant)` 這個 hash function 可滿足這些特性。

_constant_ 為下於 array 長度的質數，用 double hashing 的話，不同 key 可能 hash 出相同的 index，但會 hash 出不同的步距，而步距的區間會是 1 到 constant 的數字。一般而言 open addressing 都採用 double hashing。

#### Table Size a Prime Number

double hashing 的 array 長度必須是質數，否則 probe 會永遠在特定的幾格中循環而不會檢查到其他格是否為空的可放入資料，比方 array 長度是 15，有某個 key hash 後為 0，而 hash 後得到的步距是 5，則 probe 的格子會是 0、5、10、0、5、10 一直循環下去，即使其他格是空格也不會被檢查到。如果 array 長度改為 13，則 probe 的格子會是 0、5、10、2、7、12、4、9、1、6、11、3...，最終 array 中每個格子都會被檢查到，因為質數無法被其他數整除。

#### Java Code

請參照：[https://github.com/blackdiz/datastructrues\_and\_algorithms\_in\_java/blob/master/src/main/java/chapter11/doublehash/DoubleHashingHashTable.java](https://github.com/blackdiz/datastructrues_and_algorithms_in_java/blob/master/src/main/java/chapter11/doublehash/DoubleHashingHashTable.java)

