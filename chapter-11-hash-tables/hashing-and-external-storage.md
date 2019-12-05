# Hashing and External Storage

檔案是分散成數個包含資料的 block 的形式儲存在磁碟上，而存取 block 耗費的時間比在記憶體中處理資料的時間來得長，所以在思考外部儲存機制時最重要的考量是如何將 block 的存取降到最少。

另一方面外部儲存裝置的每 byte 成本不高，所以可以使用大量的空間，藉由空間換取效能，而 hash table 可以提供這樣的效果。

#### Table of File Pointers

external hashing 的主要特性是用 hash table 儲存 block number，這個 hash table 有時被稱為 index。hash table 可以儲存在記憶體中，如果太大則會儲存在磁碟上，每次只讀取需要的部分到記憶體中。

#### Non-Full Blocks

假設 block 大小為 8,192 bytes，而 1 筆資料的大小是 512 bytes，因此 1 個 block 可以儲存 16 筆資料，hash table 中每個 entry 都各別指向 1 個 block。

如果檔案中有 100 個 block，則記憶體中的 hash table 就儲存指向這些 block 的指標，所以會是 0 到 99，容量為 100。

在 external hashing 時，很重要的一點是 block 不要存滿資料，因此以這個例子來說平均每個 block 只會儲存 8 筆資料，某些 block 會多一些，某些少一些。所以整個檔案約有 800 筆資料。

資料 key 的 hash 值相同表示儲存在同一個 block 中，要找到特定 key 的資料，首先先對 key 做 hash，hash 值即為 hash table 的 index，用 index 取到 block number 後讀取該 block 再寫入資料。

這個過程很有效率，因為只需要 1 次的 block 存取就可以取到需要的資料，缺點是會浪費磁碟空間，因為有一半的空間是不存資料的。

要實作這個機制，我們必須慎選 hash function 和 hash table 的大小以讓 key 的 hash 值儘可能不要相同，以此例來說最好平均只有 8 筆資料的 key 的 hash 值會相同，這樣才能讓每個 block 儲存的資料數都不超過一半。

#### Full Blocks

即使有良好的 hash function，block 還是會有存滿資料的時候，這種時候就可以用之前討論處理 collision 的方式來處理。

用 open addressing 處理的話，如果新增資料時發現 block 已滿則會開始找另一個 block 儲存，如果是用 linear probing 就會使用下一個 block，或者是用 quadratic probe 或 double hashing 找。

用 separate chaining 處理的話，會建立另一個 overflow block，當原本的 block 滿的時候，新資料就會寫入 overflow block。

要儘量避免 full block 出現，因為這樣需要額外的 block 存取而加倍存取時間。



