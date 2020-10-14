# Anagrams

**anagram** 表示將某個英文字改變字母的排列順序，比方 cat 可以排出:

* cat
* cta
* atc
* act
* tca
* tac

如果字母沒有重複的話，我們可以發現排序組合的數量剛好是字母數的階乘，如 cat 的可能組合數即為 3 \* 2 \* 1 = 6。

假如要寫程序列出一個有 N 個字母的詞所有可能的排序組告的話，一種方法是:

1. 重排首字以外右方的字母，直到右方字母回到原本順序
2. 將所有字母左移一位，首字則移到最右
3. 重複 N 次，直到回到原本的順序

程式碼請參照：[https://github.com/blackdiz/datastructrues\_and\_algorithms\_in\_java/blob/master/chapter6/anagram/Anagrams.java](https://github.com/blackdiz/datastructrues_and_algorithms_in_java/blob/master/chapter6/anagram/Anagrams.java)

