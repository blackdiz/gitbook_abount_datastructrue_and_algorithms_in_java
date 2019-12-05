# Insertion

新增 node 時首先將新 node 附加在 array 最末端，接著和 parent 比較，如果 key 比 parent 大就和 parent 交換位置，重複步驟往 heap tree 的上方移動，直到新 node 介於 key 比它大和 key 比它小的 node 之間的位置。

往 heap tree 的上方移動比刪除時往下方移動簡單，因為只需要和 parent 比較，而 parent 只有 1 個，不像往下方移動有 2 個 child 時需要特別和較大 key 的 child 交換。

如果刪除 1 個 node 後再新增同樣 key 的 node，heap tree 並不會長得一樣，heap tree 的結構是按新增元素的順序而定，只可以確定一定滿足 heap condition。

