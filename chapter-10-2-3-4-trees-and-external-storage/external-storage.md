# External Storage

#### Accessing External Data

截至目前為止我們討論的資料結構都是立基於資料是存放在記憶體中，但很多時候資料量會大至無法全部放入記憶體，這種時候通常就會選擇存在磁碟中。

磁碟的容量通常比記憶體大很多，此外還有另一個優點就是持久性，當電源關閉時，記憶體內的資料就消失了，但存在磁碟中的則不會。

磁碟的缺點是速度上比記憶體慢很多，比方一個電話簿資料它存放 500,000 筆電話資料，每筆資料中有姓名、地址、電話號碼等。如果一筆資料的大小是 512 bytes，總容量就是 500,000 \* 512 bytes，也就是 256 M，我們要如何設計一個結構可以快速的查詢、新增、刪除資料呢 ?

要找到答案必須考量 2 個因素：

* 磁碟比記憶體慢很多
* 必須同時存取多筆記錄

#### Very Slow Access

記憶體是依賴電流存取，所以可以在微秒內存取資料。磁碟則是把資料放在磁盤上，要存取資料必須先把讀寫頭移動到正確的磁軌，這個動作是靠馬達這種機械式移動，所以必須花費數毫秒。當找到正確的磁軌後，讀寫頭要等含有資料的部分旋轉到定位，平均而言要等磁盤轉半圈才能就定位，假設 1 分鐘可以轉 10,000 圈，大約要耗費 3 毫秒以上才可以開始讀取資料。等到完全就定位後，讀寫頭才能開始真正讀寫資料，這又要花上數毫秒。

因此，通常磁碟的存取時間大約是 10 毫秒左右，比記憶體慢上 10,000 倍。雖然科技進步會加速存取時間，但記憶體的存取速度進步更快，所以兩者間的差距可能會愈來愈大。

#### One Block at a Time

當讀寫頭就定位開始讀寫資料時，磁碟可以一次快速傳輸大量資料到記憶體中，因此為了簡化控制機制，資料是以一區塊一區塊的方式儲存在磁碟中，這個區塊通常稱作 **block** 、 **page** 、 **allocation unit** 等，這裡以 block 稱之。

磁碟一次讀寫的最小單位就是 1 block，而 block 的大小則依不同作業系統、磁碟大小等因素而異，但通常是 2 的冪，以上述的電話資料為例，假設 1 block 是 8,192 bytes \(2 ^ 13\)，電話資料就需要 256,000,000 / 8,192 個 block，也就是 31,250 個 block。

當程式要讀取 100 bytes 的資料時，系統會讀取 1 個 block 也就是 8,192 bytes，然後將要求的 100 bytes 留下後丟掉其他資料再回傳，因此程式如果能一次讀取數個完整的 block 可以最佳化每次讀寫的效率。

假設每筆電話資料的大小是 512 bytes，每個 block 可以儲存 8,192 / 512 = 16 筆資料，所以要達到最佳效率的話重點在於一次讀取 16 的倍數筆資料。此外讓資料的大小是 2 的倍數也有助於提升效率，因為這樣可以讓資料剛好放入 block 中。

因為讀寫頭在讀取 block 的速度相當快，大約只要幾毫秒，所以如果每次都讀寫 block 中的所有資料，block 愈大效率愈好。

#### Sequential Ordering

一種儲存電話資料的方式是按某個 key 值做排序，比方依英文姓氏的字母順序排。

#### Searching

要在循序排序的檔案結構中查詢特定姓氏的資料，我們可以用 binary search。首先讀取位於檔案中間的 block，這樣會將該 block 內的 16 筆資料一次讀到記憶體中。如果這些資料的 key 值比較小，那我們就再讀取 3 / 4 位置的 block，如果較大就讀取 1 / 4 位置的 block，這樣反覆直到找到想要的資料。

在記憶體中執行 binary search 要比對 key 值 log2N 次，所以 500,000 筆資料約是 19 次，假如每次比對用了 10 微秒，總共花費 190 微秒是相當快的速度。

然而我們的資料是儲存在磁碟中，因為磁碟讀取 block 比在記憶體中的 16 筆資料中搜尋更耗時，所以磁碟的存取次數比處理的資料筆數要來得重要。

因為我們一次存取 1 個 block，而 block 的數量比資料筆數少，如果電話資料有 31,250 個 block 需要 log2N 次存取約 15 次。

在 binary search 剛開始時，將多筆資料讀入記憶體中處理在速度上的幫助不大，因為下一次存取的記錄會在另一個 block 裡，但隨著愈來愈接近目標時，下次存取的記錄可能和這次存取的記錄在同一個 block 中，所以就不需要多一次 block 的存取，因此磁碟的存取次數會比 15 次少，約末 13 次花費 130 毫秒也就是 1 / 7 秒，雖然比記憶體慢但仍不差。

#### Insertion

在循序排序的檔案中新增、刪除資料的效率比搜尋差上不少。因為資料是排序的，所以新增、刪除都需要移動平均約一半的 block。

移動 1 個 block 需要 2 次的存取，1 次讀 1 次寫。當找到資料的插入點時，首先會把含有插入點的 block 讀到記憶體中，將 block 最後 1 筆資料暫存起來，接著移動記錄在 block 中的位置讓新的記錄可以插入，完成後再將 block 寫回檔案中。接著讀入第 2 個 block，暫存它的最後 1 筆記錄，將其他的資料都移動 1 個位置後，把前 1 個 block 暫存的資料寫到第 2 個 block 的開頭位置，再把 block 寫回檔案。這樣反覆進行一直到位於插入點 block 後方的 block 都重寫過。

如果有 31,250 個 block 平均要讀寫 15,625 個 block，以讀寫 1 個 block 花費 10 毫秒來看，新增 1 筆記錄要 5 分鐘。

另一個循序排序的問題是只能根據 1 個 key 快速搜尋，以我們的例子而言是用英文姓氏做為 key 排序，假設我們是以電話號碼為 key 搜尋就無法使用 binary search 而必須遍歷所有的 block，平均需要讀取一半的 block 才能找到也就是要花費 2.5 分鐘，對一個簡單的搜尋來說效率非常差。

#### B-Trees

因此我們需要使用不一樣的結構儲存在磁碟上，tree 的結構能快速地搜尋、新增和刪除，而用在磁碟的資料儲存上的 tree 和 2-3-4 tree 類似，但每個 node 的元素更多，稱為 **B-tree** ，由 R. Bayer 和 E. M. McCreight 在 1972 首次用以做為外部儲存的結構。

#### One Block Per Node

因為磁碟一次存取 1 個 block 是最有效率的，而在 tree 中，存放資料的單位是 node，所以很自然地可以把 block 等同為 node，這樣讀取 1 個 node 就可以在最短時間內讀取到最多的資料。

那我們可以在 node 中存放多少資料 ? 如果只看資料本身，1 個 8,192 bytes 大小的 block 可以儲存 16 筆 512 bytes 的資料。但如果用 tree 的結構儲存，每個 node 還需要儲存和其他 node 的 link，在記憶體中的話，這些 link 是以 reference 的方式記錄相關聯的 node，而在磁碟上的話，link 是記錄檔案中相關聯的 block number。我們可以用 int 存放 block number，1 個 int 的大小是 4 byte 也就是足夠儲存 block number 超過 2 億的數字，對一般檔案來說綽綽有餘。

但加上 block number 後，我們就不能在 block 中儲存剛好 16 筆記錄，必須空出空間來記錄 child node 的 link。我們可以直接變成儲存 15 筆記錄用多出來的空間儲存 link，但最有效率的方式是每個 node 儲存偶數筆資料，所以我們改為把每筆資料的大小降為 507 bytes，16 筆資料下會有 17 個 child，因此 child link 需要 17 \* 4 = 68 bytes，加總後即是 507 \* 16 + 68 = 8,180 bytes 留下 12 bytes 的空間。

由於每個 node 內的資料是依 key 排序，所以 B-tree 的結構和 2-3-4 tree 相同，只差在每個 node 可以儲存的元素和 child link 比較多。

#### Searching

搜尋的步驟和 2-3-4 tree 相同，首先將含有 root 資料的 block 讀到記憶體中，比對 block 內的所有資料，當比對到大於 key 的資料時就沿著介於該筆資料和前 1 筆資料之間的 child link 讀取下筆 block，一直到讀取到所要的資料或者讀取到 leaf node 仍找不到就表示沒有該筆資料。

#### Insertion

B-tree 新增資料的步驟比較像是 2-3 tree 而非 2-3-4 tree。在 2-3-4 tree 中，許多 node 都沒有存滿元素，因為  node split 永遠產生只含有 1 個元素的 node，這在 B-tree 的應用上無法達到最佳化。

在 B-tree 中儘量讓 node 存滿資料是相當重要的，這樣每次磁碟存取才可能存取到最大量的資料，因此新增資料的步驟和 2-3-4 tree 不同：

* node split 時平均分散資料，一半移到新建的 node，一半留在原 node
* node split 和 2-3 tree 相同由下往上執行
* 和 2-3 tree 相同，新增的資料先和舊資料排序一起排序才選出中間要移到 parent 的資料

在這個過程中，每個 node 至少都會儲存一半容量的資料以最佳化 B-tree 的效率。

#### Efficiency of B-Trees

因為每個 node 儲存較多資料且每層 level 有較多的 node，B-tree 的操作非常快速，以之前的電話簿資料為例，500,000 筆資料的話，因為 B-tree 的 node 至少會儲存一半容量的資料也就是 8 筆記錄同時會有 9 個 child link，因此 tree 的高度會是 log9N ，N = 500,000，也就是 5.972 約 6 層。

 所以使用 B-tree  的話，500,000 筆資料只需要 6 次的磁碟存取就可以搜尋出結果，以 1 次存取花費 10 毫秒來說，總共要用 60 毫秒，相較於用 binary search 快了很多。

node 中的資料筆數愈多，tree 的高度就愈小。在 16 筆記錄時，B-tree 的高度為 6，相較之下，一樣 500,000 筆資料用 binary tree 的高度是 19，2-3-4 tree 是 10。如果每個 block 儲存的是上百筆資料就可以更加減少 tree 的高度來加快存取速度。

新增、刪除更是 B-tree 的優點。因為 1 個 node 可以儲存大量資料，所以很多時候新增資料時不需要 node split，以電話簿的 B-tree 為例，找到插入點的 node 需要 6 次磁碟存取，接著需要 1 次的寫入將新資料寫進 block 中，因此總共只需要 7 次存取。如果新增時需要 node split，需要 1 次讀取讀入要 split 的 node，移除一半的資料後用 1 次寫入寫回磁碟，另一半資料需要 1 次寫入寫到新的 node，接著用 1 次讀取讀入 parent，加入要移進的資料後用 1 次寫入寫回磁碟，整個過程有 5 次存取，加上找插入點的 6 次存取，相較於循序排序的結構要花上 500,000 次存取是極大改進。

有些版本的 B-tree 只有 leaf node 儲存實際資料，而非 leaf node 只記錄 key 和 block number，這可能加快操作速度，因為 node 可以記錄更多的 block number 也就是更多的 child link，因此 tree 的高度會更小，存取次數就可以更少，但實作上就會更複雜，因為必須區分出非 leaf node 和 leaf node。

#### Indexing

另一種加快存取速度的方法是循序儲存資料，但用另一個檔案記錄資料的 index。index 內儲存的是 key 和 block 成對的列表，並以 key 排序。以之前的電話簿為例，我們有 500,000 筆 512 bytes 大小的資料，每個 block 儲存 16 筆，共用了 31,250 個 block。如果搜尋的 key 是用英文姓氏，則 index 的每筆資料稱作 **entry** 包含：

* key，比方 Jones
* Jones 資料所在的 block number，從 0 到 31,249

如果 key 用 28 bytes 的字串儲存，block number 用 4 bytes 的 int 存，index 內每筆 entry 的大小就是 32 bytes，是原本直接儲存電話簿資料的 1 / 16。

index 內的 entry 以 key 循序排列，而實際的資料則可以任意順序儲存，所以新的資料可以直接附加在檔案末端，這樣實際資料就是以新增的順序排序。

#### Index File in Memory

因為 index 比儲存實際資料的檔案要小很多，甚至可以直接將整個 index 讀進記憶體中。以電話簿的 index 為例，每筆 entry 是 32 bytes，500,000 筆資料的 index 會是 32 \* 500,000 bytes = 1.6 M，對現今的電腦來說要全部存放在記憶體中毫無問題。所以 index 可以儲存在磁碟中，但當程式啟動時就將它讀取在記憶體裡，之後所有 index 的操作都在記憶體中進行，一段時間後再將更新後的 index 寫回磁碟中。

#### Searching

將 index 讀取到記憶體內的做法可以讓操作變得更快，以搜尋來說，在 index 內用 binary search 找到資料對應的 entry 要 19 次記憶體存取，記憶體每次存取花費 20 微秒，因此一共用了 4 / 10,000 秒，接著因為已經知道資料所在的 block number，所以只要 1 次磁碟存取也就 10 毫秒讀取資料。

#### Insertion

在已經將 index 讀取到記憶體的前提下，要新增資料時需要 2 個步驟，首先將完整的資料寫到儲存資料的檔案中，再把 key 和 block number 寫到 index 裡。

因為 index 內的 entry 是依 key 循序排序儲存的，所以我們需要平均需要移動一半的資料來寫入包含新資料的 key 和 block number 的 entry。在記憶體中移動 1 個 byte 要 2 微秒，500,000 筆 32 byte 的 entry 的話要移動 250,000 次，一共要用 250,000 \* 32 \* 2 微秒等於 16 秒的時間 \(如果直接以循序排序儲存資料的話要花費 5 分鐘新增 1 筆資料\)。

如果我們用 binary tree、2-3-4 tree 或 red-black tree 的結構儲存記憶體中的 index 可以縮短很多新增和刪除的存取資料。

不論用什麼方式儲存 index 都會比直接循序排序儲存資料的方式快上許多，某些情況下甚至比用 B-tree 直接儲存資料的方式還快。

而要把完整的資料寫到儲存資料的檔案中的話，因為我們可以直接將新資料附加在檔案的末端，通常是讀取檔案最後 1 個 block 到記憶體中，附加上新資料後再寫回磁碟，因此只需要 2 次的磁碟存取。

#### Multiple Indexes

用 index 的另一個好處是可以針對不同的 key 建立不同的 index 檔，比方 1 個 index 是針對姓氏，1 個是針對電話號碼等。因為 index 相較實際儲存資料的檔案不大，所以建立多個 index  不會增加太多使用空間，不過會增加在刪除資料時的複雜度，因為必須同時刪除所有 index 內對應的 entry。

#### Index Too Large for Memory

如果 index 大到無法讀取到記憶體中就必須儲存到磁碟上，而此時就很適合以 B-tree 的方式儲存 index，而資料可以用直接附加到資料檔案最末端的方式新增。

這樣的做法很有效率，因為在新增資料時直接附加在最末端的操作相當快速，而在新增 index 的 entry 時因為採用 tree 的結構，在新增、搜尋時也很快。

需要注意的因為是儲存在磁碟上，index 內每個 node 的元素是對應資料所在的 block number，而 child 是連結到 child node 所在的 block number。

#### Sorting External Files

如果資料可以完全讀取到記憶體中，我們可以用任意排序法如 quicksort 排序，因為記憶體的存取速度相當快速，但如果資料在磁碟上，quicksort 不是太好的排序法，因為我們必須儘量減少磁碟存取， 而 quicksort 必須隨機地挑選資料作為 pivot，如果剛好 pivot 不在已讀入記憶體中的資料就必須存取磁碟，最糟的狀況下每次分組排序資料時都必須存取一次磁碟。所以我們使用的方法要儘量達到循序讀取資料，因為在磁碟存取資料時循序讀取比隨機讀取要快上許多，可以 1 次存取就讀入最大量的資料。

要排序磁碟上的資料比較好的排序法是用 **external merge sort**，因為相較其他排序法，external merge sort 可以讓磁碟存取相鄰的資料處理而不是隨機存取檔案的任意部分。

external merge sort 的作法和 mergesort 類似，是一直將資料分成更小的序列稱作 **run**，直到 2 個最小的 run 排序後再合併回去，再將合併後的 2 個序列排序合併，一直到合併回完整的序列，而最小的 run 是 1 個 block。

排序分成 2 階段：

1. 首先讀取記憶體可容許容量的 block，把它們的內部資料排序 \(可以用 quicksort，或者資料量不大時可以用 insertion sort 或 shellsort\) 後寫到另 1 個檔案中，反覆進行直到所有 block 都處理過
2. 讀取這些排序過的檔案 \(即為 run\) 將它們的資料 merge sort 後產生較大的 run 寫到另 1 個檔案中，等所有 run 都處理過後，再讀取上次合併產生的 run 繼續做 merge sort 直到最終合併回 1 個 run

在過程中用另一個檔案儲存排序過的 run，在允許的情況下儘量不要修改原始檔案。

#### 2-Way External Merge Sort

**2-way external merge sort** 的 2 表示我們每次讀取用來合併排序的 run 數量，所以 2-way 表示我們每次讀取 2 個 run 合併排序成新的 run。

假設資料分散在 N 個 block，記憶體內有 B 個 buffer \(用來暫存資料的緩衝區\) 讀入的資料和準備寫出的資料，則 2-way external merge sort 的步驟是：

**Pass \#0** ：讀取 B 個 buffer 可存入大小的 block 到記憶體中，排序它們的內部資料後寫回到磁碟另一個檔案，此即為 1 個 run，反覆進行直到所有的 block 都轉換成 run 為止

**Pass \#1、2、3...** ：讀取 2 個 run 做 mergesort 後產生新的 run 寫回到磁碟另一檔案，所以每次產生的新 run 大小為舊 run 的 2 倍。等每個 run 都處理過後，這次 pass 就結束，下次 pass 開始同樣再讀取 2 個上一次 pass 產生的 run 做 mergesort 後產生新 run 寫回去磁碟，重複這個步驟直到最後合併回一個 run 為止。過程中需要 3 個 buffer，2 個 buffer 用以暫存讀入的 run 的資料，1 個用來暫存準備寫到磁碟的資料

因為每次讀取 2 個 run 合併一直到合併回 N 個 block 為止，所以需要 _ceiling\(log2N\)_ 次合併，加上 pass \#0 將每個 block 預先排序的步驟，一共需要 _1 + ceiling\(log2N\)_ 次 pass。

而每次 pass 中每個 block 都會被讀入 1 次合併排序後再寫出 1 次，所以整體的 I/O 成本為 _2N \* pass次數_ 。

上述的步驟中用到的 buffer 數 B = 3，如果 B &gt; 3 不會增加效率，因為我們依然每次 pass 依然必須讀寫 1 次每個 block，所以單純增加 B 不會減少 I/O 成本。

#### Double Buffering Optimization

降低 I/O 成本的一個方法是預讀 \(**prefetch**\) ，在 mergersort 目前的 run 時，prefetch 下次要讀入的 run 儲存在另外的 buffer 中，等到目前的 run 處理完後就接著處理 prefetch 進來的 run 並 prefetch 下次的 run 。

這樣做的好處是可以減少等待 I/O 讀取的時間，在 mergesort 時是 CPU 計算，此時 I/O 是閒置的，等到讀取 run 變成是 I/O 處理但 CPU 閒置，因此我們在 CPU 做 mergesort 時不讓 I/O 閒置，先 prefetch 下次的 run 這樣等到這次的 run 處理完後，CPU 就不用等待 I/O 讀取 run。因為 OS 在處理 I/O 是非同步 \(asynchronous\) 的，所以不會卡住目前 mergesort 的 thread。

#### General External Merge Sort

因為每次 mergesort 時，有 1 個 buffer 是用來暫存寫出資料，所以每次可讀入處理的 run 為 B - 1 個，則我們可以得到 external merge sort  的一般公式：

**Pass \#0** ：當用 B 個 buffer 時，可以產生 _ceiling\(N / B\)_ 個 run，每個 run 都是已排序，大小為 buffer 的大小

**Pass \#1、2、3...** ：每次讀入 B - 1 個 run 合併排序後寫出，直到合併回一個完整的 run

這個做法稱作 **K-way merge** ，K = B - 1，因此 external merge sort 的合併次數是看每個合併時用的讀取 buffer 數也就是 B - 1 可以處理完 pass \#0 產生的 run 數也就是 N / B，因此 pass 次數 pass \#0 + 合併的 pass 次數，即為 _1 + ceiling\(logB-1ceiling\(N / B\)\)_ 次，而 I/O 成本一樣是 _2N \* pass 次數_。

以 N = 108，B = 5 為例：

**Pass \#0** ：ceiling\(N / B\) = ceiling\(108 / 5\) = 22 所以會產生 22 個 run，最後一個 run 只有 3 個 block

**Pass \#1** ：ceiling\(run 個數 / \(B - 1\)\) = ceiling\(22 / 4\) = 6 產生 6 個合併排序後的 run，最後一個 run 只有 8 個 block

**Pass \#2** ：ceiling\(run 個數 / \(B - 1\)\) = ceiling \(6 / 4\) = 2 產生 2 個合併排序後的 run，第 1 個有 80 個 block，第 2 個有 28 個 block

**Pass \#3** ：將 2 個 run 合併排序後得到完整已排序的檔案

套用公式的話：

1 + ceiling\(logB-1ceiling\(N / B\)\) = 1 + ceiling\(log422\) = 1 + ceilin\(2.229...\) = 4

