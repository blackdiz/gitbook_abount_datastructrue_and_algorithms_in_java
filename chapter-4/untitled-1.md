# Parsing Arithmetic Expressions

解析運算式如: 2 + 3 或 2 \* \(3 + 4\) 等是用stack，但對電腦演算法來說這項任務其實不簡單，它牽扯到2個步驟:

1. 將運算式轉換成 **postfix notation** ，即後綴表示法，運算子置於運算元的後面。
2. 計算 **postfix notation**

#### Postfix Notation

一般常見將運算子置於運算元中間如: 2 + 3 的寫法稱作 **infix notation** ，而 **postfix notation** 則是將運算子放在運算元後，所以 A + B 會改為 AB+，而 A / B 則變成 AB/，更複雜一點的運算例如包含 \(\) : \(A + B\) \* \(C - D\)，則轉為: AB+CD-\*。

#### Translating Infix to Postfix

人類在解析運算式時的步驟為:

1. 由左至右讀運算式
2. 當讀到足夠的運算元和運算子後就可以進行計算，並將計算後的結果代入
3. 繼續前面2個步驟直至整個運算式結束

以 3 + 4 - 5 來說，計算步驟為:

1. 讀到 3
2. 讀到 +，合為 3 +
3. 讀到 4，合為 3 + 4，此時尚不可進行計算，因為如果下一個運算子為 \* 或 / 則表示此時 4 是和之後的運算元計算
4. 讀到 - ，此時我們可以計算 3 + 4 = 7
5. 讀到 5，合為 7 - 5
6. 已到運算式結束，所以計算 7 - 5 = 2

另一個例子 3  + 4 \* 5:

1. 讀到 3
2. 讀到 +，合為 3 +
3. 讀到 4，合為 3 + 4
4. 讀到 \* ，合為 3 + 4 \* ，所以表示 3 + 4 的運算順序在比較後面
5. 讀到 5，合為 3 + 4 \* 5，此時已經有 \* 兩邊的運算元，所以可以計算 4 \* 5 = 20，並代回運算式變成 3 + 20
6. 已到運算式結束， 所以計算 3 + 20 = 23

所以在計算時，必須確認右邊運算元後面接的運算子優先順序，如果後面的運算子優先順序大於目前運算式的運算子就表示目前運算式的計算順序是在後的。

如果加上括號的話，括號內的運算順序會提前，如: 3 \* \(4 + 5\):

1. 讀到 3
2. 讀到 \*，合為 3 \*
3. 讀到 \( ，合為 3 \* \( ，且表示後方的運算會優先
4. 讀到 4，合為 3 \* \( 4
5. 讀到 +，合為 3 \* \( 4 +
6. 讀到 5，合為 3 \* \( 4 + 5
7. 讀到 \) ，合為 3 \* \( 4 + 5 \)，此時 \(\) 已經結束，可以計用 \(\) 內的 4 + 5 = 9，並代回變成 3 \* 9
8. 計算 3 \* 9 = 27

把 **infix notation** 轉為 **postfix notation** 的步驟很類似，當讀到的是運算元時，我們馬上記下，但運算子則要等到讀到的資料足夠形成一個完整的運算式時才將運算子記下，如 A + B - C:



```text
|讀入的資料|infix notation|postfix notation|說明|
|---|---|---|---|
| A | A | A ||
| + | A + | A ||
| B | A + B | AB ||
| * | A + B * | AB | 此時不能填上+，因為讀到的是*，表示後面有順序較高的運算式 |
| C | A + B * C | ABC* | 讀到C時，剛剛讀到的*兩邊運算元都有了，所以可以填上* |
|   | A + B * C | ABC*+| 因為整個運算式已經讀完，所以留下的+可以填上|
```

#### Saving Operators on a Stack

**infix** 和 **postfix** 的運算子順序正好相反，因為第一個運算子不能馬上填上，必須等到下個運算子填上後才能輪到它，以A + B \* \(C - D\) 為例:

| 讀入的資料 | infix notation | postfix notation | stack中的元素 |
| :--- | :--- | :--- | :--- |
| A | A | A |  |
| + | A + | A | + |
| B | A + B | AB | + |
| \* | A + B \* | AB | +\* |
| \( | A + B \* \( | AB | +\*\( |
| C | A + B \* \(C | ABC | +\*\( |
| - | A + B \* \(C - | ABC | +\*\(- |
| D | A + B \* \(C - D | ABCD | +\*\(- |
| \) | A + B \* \(C - D\) | ABCD- | +\* |
|  | A + B \* \(C - D\) | ABCD-\* | + |
|  | A + B \* \(C - D\) | ABCD-\*+ |  |

可以看到 **postfix** 的運算子順序是 -\*+，和 **infix** 的 +\*- 相反，因為 \* 的優先順序大於 + ，而 - 是在 \(\) 中所以優先順序大於 \*。

這種相反的特性剛好適合用先進後出的 stack 儲存，我們將讀到的運算子暫存到 stack 中，之後就可以按相反的順序取出。

#### Java Code

請參照: [https://github.com/blackdiz/datastructrues\_and\_algorithms\_in\_java/blob/master/src/main/java/com/blackdiz/algorithm/chapter4/InfixApp.java](https://github.com/blackdiz/datastructrues_and_algorithms_in_java/blob/master/src/main/java/com/blackdiz/algorithm/chapter4/InfixApp.java)

#### Evaluating Postfix Expressions

解析 **postfix** 時，只要讀到運算子，就可以把運算子前2個運算元拿來運算後代回算式再繼續往右讀取，以 345+\*612+/- 為例:

| 讀入 | 運算 | 算式 |
| :--- | :--- | :--- |
| 3 |  | 3 |
| 4 |  | 3 4 |
| 5 |  | 3 4 5 |
| + | 4 + 5 = 9 | 3 9 |
| \* | 3 \* 9 = 27 | 27 |
| 6 |  | 27 6 |
| 1 |  | 27 6 1 |
| 2 |  | 27 6 1 2 |
| + | 1 + 2 = 3 | 27 6 3 |
| / | 6 / 3 = 2 | 27 2 |
| - | 27 - 2 = 25 | 25 |

所以我們可以只要讀到的是數字運算元就存入stack中，讀到運算子時將stack前2個元素pop出來運算後，push回stack中等待下一個運算子時再pop出來運算

#### Java Code

請參照: [https://github.com/blackdiz/datastructrues\_and\_algorithms\_in\_java/blob/master/src/main/java/com/blackdiz/algorithm/chapter4/PostFixApp.java](https://github.com/blackdiz/datastructrues_and_algorithms_in_java/blob/master/src/main/java/com/blackdiz/algorithm/chapter4/PostFixApp.java)

