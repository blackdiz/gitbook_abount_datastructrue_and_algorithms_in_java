# The Efficiency of Red-Black Trees

如同一般的 binary search tree，red-black tree 的插入、查詢、刪除的時間複雜度均為 O\(logN\)。

根據 Sedgewick 的分析，實際上查詢需要約 logN 次的 node 比較，而且絕對不會超過 2 \* logN 次，所以可以視為 O\(logN\)。

而查詢、刪除則是常數增加，因為在過程中需要 color flip 和 rotation 等步驟，平均而言需要 1 次的 rotation，但我們可以忽略常數所以時間複雜度依然是 O\(logN\) 但會比普通的 binary search tree 要慢一些。

因為多數的應用中，查詢比插入、刪除要來得多，所以用 red-black tree 不會造成太多的時間差，同時還有確保不會出現不平衡而導致時間複雜度變 O\(N\) 的情況。

