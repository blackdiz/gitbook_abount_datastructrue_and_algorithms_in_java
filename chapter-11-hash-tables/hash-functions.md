# Hash Functions

#### Quick Computation

好的 hash function 是要簡單可以快速運算出結果的，hash table 的主要優點是操作速度，如果 hash 時太慢，速度就會下降。因此 hash 中用到許多乘除運算的就不是太好的 hash function \(Java 或 C++ 中使用位元移位來快速 \* 2 或 / 2 的方法可以帶來一些速度上的好處\)。

#### Random Keys

hash 的目的是將一個區間內的 key 轉換成 index，而且要能隨機分布在 hash table 中。完美的 hash function 要能將每個 key 都對應到不同的 index，但通常只有當 key 天生就有分散性且數量不大時才有可能達成。大多數的情況下，hash function 都需要將大區間的 key 壓縮成小區間的 index。

特定資料庫中的 key 值分布情況決定 hash function 需要做的事，比方之前我們假定資料是隨機的，所以 `index = key % array size` 這樣的公式就可以符合需求，它只有一道算式，且如果 key 是隨機的則 index 也會是隨機的。

#### Non-Random Keys

然而資料通常沒有隨機性，比方汽車零件的資料庫是用零件編號做為 key，例如：033-400-03-94-05-0-535，編號有下列規則：

* 0 到 2 位：供應商編號，1 至 999，目前最多至 70
* 3 到 5 位：分類編號，100、150、200、250，以此類推，最多到 850
* 6 到 7 位：月份
* 8 到 9 位：年份，00 到 99
* 10 到 11 位：序號，1 到 99
* 12 位：是否具有毒性的標記，0 或 1
* 13 到 15 位：檢查碼，其他欄位加總後 % 100

但像這樣的 key 並不是隨機的，從 0 到 9,999,999,999,999,999 中不是任意數字的組合都是符合規則的編號，而且有些數字是不可能出現，比方供應商編的部分不可能出現大於 70 的數字，此外如檢查碼是依其他欄位的數字而計算出來的，因此必須特別處理這些編號讓它們可以變成更隨機的數字分布。

#### Use All the Data

key 的每一部分除了非屬於資料的部分如檢查碼之外都應該列入 hash 處理的範圍內，愈多資料加入 hash 計算中就愈可以讓 hash 後的結果平均分散。

總結來說要找到簡單且可以快速計算的 hash function，並使用 key 中除了非資料部分外的所有資料。

#### Use a Prime Number for the Modulo Base

通常 hash function 中會和 hash table 內的 array 長度做 % 運算，當使用 quadratic probe 或 double hashing 時，array 長度必須是質數。

但如果 key 不是隨機分布的話，不管用什麼 hash 機制，array 長度都要是質數，因為當許多 key 的因數都是 array 長度時，hash 後的 index 容易相同所以會放在同樣的位置而產生 cluster，如果 array 長度是質數就可以避免這種情形發生。

#### Hashing Strings

在之前的例子中乘 27 的指數將字串轉換成數字，例如 _cats_：

```text
key = 3 * 27 ^ 3 + 1 * 27 ^ 2 + 19 * 27 ^ 0
```

如果使用 Java 的話：

```java
public static int hashFunction1(String key) {
    int hashValue = 0;
    int pow27 = 1; // 1, 27, 27 * 27...
    for (int i = key.length - 1; i >= 0; i--) { // 由右至左
        int letter = key.charAt(i) - 96; // 取得字母的編碼
        hashValue += letter * pow27;
        pow27 *= pow27;
    }
    return hashValue % arraySize;
}
```

但 `hashFunction1` 的效率還不夠好，因為在迴圈中有 2 次的 \* 運算和 1 次的 + 運算，我們可以用 **Horner's method** 來轉換，Horner's method 指：

$$
var4 * n^4 + var3 * n^3 + var2 * n^2 + var1 * n^1 + var0 * n^0
$$

可以轉換成為：

$$
(((var4 * n + var3) * n + var2) * n + var1) * n + var0
$$

所以我們可以由最內部括號內的式子開始往外計算，因此上面的 Java 程式碼可以改寫成：

```java
public static int hashFunction2(String key) {
    int hashValue = key.charAt(0) - 96; // 最右邊字母的編碼
    for (int i = 1; i < key.length(); i++) { // 由左至右
        int letter = key.charAt(i) - 96;
        hashValue = hashValue * 27 + letter; // * 27 後加上下個字母的編碼
    }
    return hashValue % arraySize;
}
```

這樣迴圈中就只剩下 1 次 \* 運算和 1 次 + 運算。但 `hashFunction2` 沒有辦法處理超過 7 個字母的單字，因為計算中途 `hashValue` 會得到超出 `int` 可以容許的區間，即使改用 `long` 也是有其極限。

注意到 key 最後的結果一定小於 array 長度，因為我們最後會和 array 長度做 % 運算，所以 `hashValue` 不是最終的結果只是計算中途的值，因此我們可以在計算過程內每一次迴圈中就和 array 長度做 % 運算，這樣 hash 結果相同且不會造成 overflow：

```java
public static int hashFunction3(String key) {
    int hashValue = key.charAt(0) - 96;
    for (int i = 1; i < key.length(); i++) {
        int letter = key.charAt(i) - 96;
        hashValue = (hashValue * 27 + letter) % arraySize;
    }
    return hashValue %= arraySize;
}
```

這種方式通常用來處理字串的 hash 。除此之外，有些位元運算也可以用來增進效率，例如用 32 或任何 2 的指數做為底數，這樣可以用 &gt;&gt; 做 \* 運算，速度上會較快。

#### Folding

另一種 hash function 的作法是將 key 分成幾組數字然後加總，這樣可以確保 key 的每一部分都可以加到 hash 運算中。每一組的數字位數取決於 array 長度，比方 array 長度為 1,000 時，則每一組數字為 3 位數。

舉例來說，想將 9 位數的社會安全碼 hash 出長度為 1,000 的 array 的 index，若安全碼是 123-45-6789，則我們將它分組成 123 + 456 + 789 = 1368，再用 1368 % 1000 = 368 計算出 index，因為 3 位數最大數字為 999 ，所以 % 1000 後最大的 index 剛好是 999。假如 array 長度是 100 則分組成 2 位數，所以會變成 12 + 34 + 56 + 78 + 9 = 189， 189 % 100 = 89。

