# Triangular Numbers

假設有 n 個數，第 n 個數是 n + \(n -1\) + \(n - 2\) + ... + 1 所得，這樣形成的數列稱為 triangular numbers \(三角形數\)，例如: 1、3、6、10、15，第 3 個數字是 3 + 2 + 1 = 6，第 5 個數字是 5 + 4 + 3 + 2 + 1 = 15。

我們可以用圖形表現:

```text
// 排列後會形成一個三角形
o // 第 1 個數是 1
o o // 第 2 個數是 2 + 1 = 3
o o o // 第 3 個數是 3 + 2 + 1 = 6
o o o o // 第 4 個數是 4 + 3 + 2 + 1 = 10
```

#### Finding the nth Term Using a Loop

由圖形中可以看出第 n 個數就是從 n 開始加上前一行數，一直加到 1 為止，所以 我們可以用迴圈來計算出第 n 個數，如:

```java
public int triangle(int n) {
    int total = 0;
    while (n > 0) { // 計算到 n = 1 時
        total += n;
        n--;
    }
    return total;
}
```

#### Finding the nth Term Using Recursion

這個圖形從另一個角度看，每個數字都是第 n 行加上它以上所有行數的加總，如第 5 個數可以看成是 5 加上 5 以外剩下行數的加總，所以可以寫成:

```java
public int triangle(int n) {
    // sumRemainingRows(n) 可計算第 n 行剩下的行數加總
    return n + sumRemainingRows(n);
}
```

而 **sumRemainingRows\(n\)** 可以看成是計算 n - 1 的行數加總，所以可以改寫成:

```java
public int triangle(int n) {
    // sumAllRows() 可計算傳入數的總和
    return n + sumAllRows(n - 1);
}
```

但 **sumAllRows\(\)** 做的事情其實就是 **triangle\(\)** 所以可以再改成:

```java
public int triangle(int n) {
    reutrn n + triangle(n - 1);
}
```

#### Passing the Buck

這個過程就是一直將問題傳遞下去，我們要知道第 9 個數字答案需要前 8 個的加總，所以問題變成要知道第 8 個數字，但這時候變成需要前 7 個的加總，一直下去直到第某個數字不需要依賴前面的加總時才結束，如果沒有這個結束條件，那就會無限的傳遞下去。

#### The Buck Stops Here

在這裡的結束條件就是當要計算第 1 個數時，它的答案就是 1，所以不用再把問題傳遞下去，因此 **triangle** 可以寫成:

```java
public int triangle(int n) {
    if (n == 1) {
        return 1;
    } else {
        return n + triangle(n - 1);
    }
}
```

這種讓遞迴結束的條件稱為 **base case** ，每個遞迴一定要有 base case，否則就會無限一直執行下去。

#### Characteristics of Recursive Methods

遞迴有下列幾個特性:

* 它會呼叫自己
* 當它呼叫自己時會用來解決更小或更簡單的問題，比方參數愈來愈小或者數量變少
* 問題可以簡化到不用再呼叫自己就可以解決，然後開始回傳答案給前一個呼叫者

#### Is Recursion Efficient?

呼叫一個 method 會花費額外的資源，執行權需要從呼叫者轉移到被呼叫的 method，此外 method 的參數還有記錄它回傳時的目標 method 都需要放進一個 系統的 stack 中來追蹤，故以這個三角形數計算來說，迴圈版本可能比遞迴版本來得更快

另一個遞迴的消耗在記憶體，因為必須暫存每一次 method 回傳的結果在系統 stack 中，當結果資料量太大時就會造成 stack overflow 的問題。

所以遞迴常常是當可以簡化問題的處理時才使用，而不是為了效率因素。



