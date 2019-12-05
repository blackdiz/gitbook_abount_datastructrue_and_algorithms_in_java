# The Huffman Code

**Huffman code** 是用 binary tree 來壓縮資料的方法，由 David Huffman 在 1952 發明。

#### Character Codes

一般文字在電腦中用 byte 表示，如 ASCII 用 1 byte 而 Unicode 使用 2 byte，因此每個文字使用的 bit 數量都相同，例如在 ASCII，每個字母都需要 8 bit 的容量：

| 字母 | 10 進位 | 2 進位 |
| :--- | :--- | :--- |
| A | 65 | 01000000 |
| B | 66 | 01000001 |
| C | 67 | 01000010 |

以純文字而言，最常使用的壓縮方法就是減少最常出現的文字所需要的 bit 數，比方在英文中，E 常常出現，我們可以改用 01 表示 E 就可以省下空間，但如果使用 2 bit 我們就只有 4 種組合可以表示 4 個最常出現的字母。此外，這些組合不可以是和其他文字 2 進位開頭相同，否則我們無法知道它是那個字母，比方 E 為 01，而 X 為 01011000，當我們拿到 01011000 時，我們在解碼時就無法確認開頭的 01 是指 E 或是它只是 X 的起頭而已，所以在挑選壓縮的 bit 組合不能和其他 bit 組合的開頭重覆。

另外在不同訊息中，最常出現的字母不一定相同，比方如果是 Java 的程式碼，那 ; 出現的次數可能就比 E 多，解決這個問題的方法是針對訊息計算裡面每個字母出現的次數，次數愈多的那些字母應該用愈少 bit 表示，比方 **SUSIE SAYS IT IS EASY.** ，我們可以統計出：

| 字母 | 次數 | bit |
| :--- | :--- | :--- |
| A | 2 | 010 |
| E | 2 | 1111 |
| I | 3 | 110 |
| S | 6 | 10 |
| T | 1 | 0110 |
| U | 1 | 01111 |
| Y | 2 | 1110 |
| 空格 | 4 | 00 |
| . | 1 | 01110 |

我們用 10 表示 S，00 表示空格，而 11 和 E 開頭重覆，01 和 T 開頭重覆，所以都不能用。相同地如果用 3 個 bit 表示可以組合出 000、001、010、 011、100、101、110、111 共 8 種組合，我們用 010 為 A，110 為 I，而 10 和 00 的開頭已和 S 、空格重覆所以不能用，011 則和 U、. 重覆，111 和 E、Y 重覆，所以最終只有　A 和 I 的組合可以用。

最後，原句就可以轉換成 10 01111 10 110 1111 00 10 010 1110 10 00 110 0110 00 110 10 00 1111 010 10 1110 01110，當然實際上字母 bit 間的是沒有空格的。

#### Decoding with the Huffman Tree

解碼時需要用到稱作 **Huffman tree** 的 binary tree，tree 中每個 leaf node 的資料是字母，而出現次數愈高就會愈靠近 tree 的頂端：

// TODO 待補圖

在解碼時，只要遇到 0 就往 left child，遇到 1 往 right child 直到到達 leaf node 就是對應的字母。

#### Creating the Huffman Tree

建立 Huffman tree 的步驟為：

1. 首先為訊息中的每個字母建立 node，node 中含有字母本身和字母在訊息中出現的次數。
2. 將這些 node 插入 1 個 priority queue 中，出現次數愈少的優先順序愈大
3. 先取出前 2 個 node，將它們的出現次數相加後建立新 node 做為它們的 parent，parent 內只有出現次數的加總沒有對應的字母
4. 將這個新的 tree 插入 priority queue 中
5. 重複步驟 3、4， queue 中的 node 會變少，直到 queue 中只剩下 1 個 node 為止，最後的 node 就是 tree 的 root

#### Coding the Message

關於編碼訊息的部分，我們要做一張編碼表記錄每個字元對應的 Huffman code，假設我們的系統只能使用大寫英文字母外加空格和句點，我們可以由 0 開始為它們排序建立一張編碼表，比方 A 為 0，B 為 1，一直到 Z 為 25，而空格是 26 ，句點則是 27。我們的編碼表可以是一個陣列，index 是順序，而元素是字母對應的 Huffman code，比方 index 0 的元素就是 A 的 Huffman code 010，我們不需要每個字母都有對應的編碼只要有出現在訊息中的即可，在編碼訊息時，只要從編碼表中取出對應的 Huffman code 串接起來就可以了。

PS. 以 Java 而言，char 本身可以做為陣列的 index \(Java 會轉換成 int\)，所以可以用：

```java
char[] codeTable = new char[27];
char['A'] = '010';
....
```

這樣方式建立。

#### Creating the Huffman Code

建立 Huffman code 方式和解碼的規則類似都是應用 Huffman tree，因為每個字在 Huffman tree 中都是 leaf node，所以我們從 root 出發，記錄下每條從 root 到 leaf node 的 path，只要往 left child 移動的就記上 0，往 right child 移動的就記上 1，每條 path 到 leaf node 記錄的組合就是該字母的 Huffman code。

