# Removal

刪除操作表示移除含有最大 key 的 node，而這個 node 一定是 root 所以要移除它很容易，因為 root 永遠都是 array 中第 1 個元素，所以 `max node = heap array[0]` 。

問題在於移除 root 後，heap tree 就不再是完整樹，它現在出現 1 個空格。我們可以直接把所有元素往前移動 1 格來填滿空格，但有比較快的方法：

1. 移除 root
2. 將最末端的 node 移動到原 root
3. 將該最末端的 node 往後移動直到介於 key 比它大和 key 比它小的 node 之間的位置

步驟 2 補上 root 的空格將 heap tree 回復到完整樹的狀態，而步驟 3 則保持 heap condition 的每個 node 都要比它的 child 大的要求。

在步驟 3 移動 node 時，是往 child 中 key 較大的 child 那邊交換位置，因為如果往 key 較小的 child 交換，則交換後 key 較小的 child 的會變成較大 key 的 child 的 parent 而違反 heap condition：

```text
  30
 /  \
60  70

// 交換時，70 較大所以和 70 交換位置

  70
 /  \
60  30

// 如果和 60 交換，則交換後 60 變成 70 的 parent 違反 heap condition

  60
 /  \
30  70
```

