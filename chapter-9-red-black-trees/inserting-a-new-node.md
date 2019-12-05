# Inserting a New Node

除了 root 外，新插入的 node 都是紅色，因為插入紅色 node 比黑色 node 更不容易違反 red-black rule，如果新插入的紅色 node 是接在黑色 node 下沒有違反任何規則，既不會讓相鄰的 node 都是紅色 \(違反規則 3\)，也不會改變 black height \(違反規則 4\)，頂多有一半的機會會接在紅色 node 下而違反規則 3，但如果新插入的是黑色 node 則一定會改變那條 path 的 black height，所以我們選擇都插入紅色。

#### Preview of the Insertion Process

假設 X 是在插入新 node 的過程中某個違反規則的 node，它可能是新 node 本身也可能是相鄰 2 個紅色 node 的 child，而 P 是 X 的 parent，G 為 P 的 parent。

在從 root 往下找尋插入點時，如果遇到 1 個黑色 node 有 2 個紅色 node 的情況就做 color flip，而 color flip 後假如出現 parent 和 child 均為紅色的情況就用 rotation 修正，反復一直向下執行直到找到插入點。

在插入新 node X 後，假設 P 為黑色 node 就完成步驟，如果 P 為紅色則以顏色轉換和 rotation 修正。

#### Color Flips on the Way Down

color flip 指將 parent 和 child 的顏色換成相反的，黑換成紅，紅換成黑。在插入新 node 的過程中，如果遇到 1 個黑色 node 有 2 個紅色 child 的情形我們就要做 color flip 將 parent 變成紅色，child 變成黑色，除非 parent 是 root 那就維持黑色 \(規則 2\)。

假設有 3 個 node，P 為黑色的 parent，它的 2 個紅色 child 分別是 X1、X2：

```text
  P (B)
 / \
X1 X2 (R)

// color flip 後

  P (R)
 / \
X1 X2 (B) 
```

在 color flip 前，P 為黑色，所以從 root 經過 P 到任何 P 底下的 leaf 或 null node 的 black height，P 都貢獻了 1 個黑色 node。color flip 後，雖然 P 變成紅色，但因為 X1、X2 變成黑色，所以不論經過 X1 或 X2 的 path 的 black height 都維持不變而不會違反規則 4。

為什麼要 color flip ? 因為這樣比較不容易違反規則 3，如果 X1、X2 維持紅色，而插入的新的紅色 node 是接在兩者之一下面，就會變成相鄰的紅色 node 而違反規則 3，而在 color flip 後我們將 leaf 都變成黑色，這樣就消除了違規的可能性。

上述提到如果是 root 下的 child 為紅色，因為規則 2 的關係，我們只把 child 變成黑色，而 root 保持黑色，因為所有的 path 一定從 root 開始且一定經過 root 的 child，所以我們同時對每條 path 的 black height 加 1，因此所有 black height 依然相等而不會違反規則 4。

除了 color flip 外，插入新 node 的步驟和 binary search tree 相同，從 root 開始依 node 的 key 和新 node 的 key 的大小，新 node 的 key 小於 node key 就往左，大於或等於 node key 就往右，直到到達 leaf node 後再依和 leaf node 的 key 比較大小的結果接在 left child 或 right child。

#### Rotations After the Node Is Inserted

雖然我們可以用 color flip 避免當 parent 為黑色 child 都為紅色時造成相鄰 node 均為紅色的問題 \(規則 3\)，但有可能 parent 為黑色而只有 1 個紅色 child，此時我們不會 color flip，但如果新插入的 node 是接到紅色 leaf child 下就會違反規則 3，因此在插入新 node 後我們要用 rotation 修正這種情形。

假設 X 為新插入的 node，P 是 X 的 parent，G 是 X 的grandparent \(也就是 P 的 parent\)，則可能有 4 種組合：

```text
// P 為 G 的 left child，X 為 G 的 outside grandchild
    G
   /
  P
 /
X

// P 為 G 的 left child，X 為 G 的 inside grandchild
    G
   /
  P
   \
    X

// P 為 G 的 right child，X 為 G 的 outside grandchild
    G
     \
      P
       \
        X

// P 為 G 的 righ child，X 為 G 的 inside grandchild
     G
      \
       P
      /
     X 
```

如果 X 和 P 是在同一邊 \(同為 left child 或 right child\)，那 X 為 G 的 outside grandchild，如果不同邊則 X 為 G 的 inside grandchild。

而 X 和 P、G 可能的組合有 3：

1. P 是黑色
2. P 是紅色，X 為 G 的 outside grandchild
3. P 是紅色，X 為 G 的 inside grandchild

#### Possibility 1: P Is Black

如果 P 是黑色則不會產生任何問題，因為 X 一定是紅色，所以既不會出現相鄰的紅色 node \(規則 3\)，也不會因新增了黑色 node 而改變 black height \(規則 4\)，因此不用額外的操作，所以插入的流程到此結束。

#### Possibility 2: P Is Red and X Is Outside

如果 P 是紅色，而 X 的插入位置是 G 的 outside grandchild，我們須改變 node 的顏色同時做 1 次 rotation：

1. 轉換 G 的顏色
2. 轉換 P 的顏色
3. 以 G 為 top node 向 X 所在位置的反方向旋轉，也就是如果 X 為 left child 則向右旋轉，right child 則向左旋轉

#### Possibility 3: P Is Red and X Is Inside

當 P 是紅色，而 X 的插入位置是 G 的 inside grandchild 時，我們必須做 2 次 rotation，第一次將 X 由 inside grandchild 變成 outside grandchild，這樣我們就把結構變成 possibility 1 的情況，所以接著如同 possibility 1 再以 G 為 top node 做反方向的旋轉。此外，為了符合規則，某些 node 需要改變顏色，因此整個步驟是：

1. 轉換 G 的顏色
2. 轉換 X 的顏色
3. 以 P 為 top node，往 X 的反方向旋轉，如果 X 是 left child 則向右旋轉，right child 則向左旋轉
4. 以 G 為 top node 向 X 所在位置的反方向旋轉，也就是如果 X 為 left child 則向右旋轉，right child 則向左旋轉

#### What About Other Possibilities

假設 X 有 sibling S 即 P 的另一個之前插入的 child，在 P 為黑色的情況下不會違反任何規則。而如果 P 為紅色，因為相鄰的 node 不能都是紅色 \(規則 3\)，所以 P 的 2 個 child 必定為黑色，而因為 black height 要保持相同所以不能只有 1 個 S 為黑色，所以不可能出現 P 為紅色，而只有 1 個 child 的狀況。

另一個變數是 G 除了 P 以外有另一個 child U，一樣地如果 P 為黑色不會違反規則，所以不特別討論。如果 P 為紅色，那 U 必定要是紅色，否則由 G 到 P 和 由 G 到 U 的 black height 就不會相同，而 G 一定是黑色，不然會違反相鄰 node 不可均為紅色 \(規則 3\)，但如果 G 為黑色，而 P 和 U 為紅色，則我們就會做 color flip 轉成 G 為紅色，P、U為黑色，因此在有 U 的情況 P 不可能為紅色。

#### What the Color Flips Accomplished

如果 rotation 和轉換 node 顏色造成 tree 的其他部分有不符合規則的狀態出現，我們可能要回過頭來往 root 開始重新修正整個 tree 的狀態，但藉著在插入時從 root 開始一路往下遇到 parent 為黑色且 2 個 child 均為紅色時就做 color flip 的步驟，就不可能發生這種狀況，所以只要針對要新插入的 node 處理就可以了，也因為這樣，red-black tree 比其他平衡樹如 AVL tree 來得較有效率，因為只需要由 root 往 leaf 一次即可。

#### Rotations on the Way Down

color flip 可能會形成違反規則 3 的狀況：

```text
   R
  / \
 B   B
    / \
   R   R

// color flip 後，出現相鄰的紅色 node

   R
  / \
 B   R
    / \
   B   B
```

而在插入的過程中 rotation 可以解決這個問題，rotation 時可能出現 2 種狀況，錯誤的 child node 是 outside grandchild 或 inside grandchild。

#### Outside Grandchild

修正的步驟和插入新 node 的修正步驟相同，假設錯誤的 child node 為 X，parent 為 P，P 的 parent 為 G，我們需要 2 次的顏色轉換和 1 次 rotation：

1. 轉換 G 的顏色
2. 轉換 P 的顏色
3. 以 G 為 top node 向 X 所在位置的反方向旋轉，也就是如果 X 為 left child 則向右旋轉，right child 則向左旋轉

#### Inside Grandchild

同樣地，設錯誤的 child node 為 X，parent 為 P，P 的 parent 為 G，我們需要 2 次的顏色轉換和 2 次 rotation：

1. 轉換 G 的顏色
2. 轉換 X 的顏色
3. 以 P 為 top node，往 X 的反方向旋轉，如果 X 是 left child 則向右旋轉，right child 則向左旋轉
4. 以 G 為 top node 向 X 所在位置的反方向旋轉，也就是如果 X 為 left child 則向右旋轉，right child 則向左旋轉

#### 

