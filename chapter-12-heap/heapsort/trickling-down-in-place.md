# Trickling Down in Place

在 heapsort 時如果新增 N 個元素到 heap 中會執行向上移動的步驟 N 次，但我們可以將這個過程用另一個方式取代，在新增元素時先隨機新增到 heap array 中，接著只要執行 N / 2 次的向下移動步驟就可以得到正確的 heap 結構，這樣可以在速度上取得一點小增進。

#### Two Correct Subheaps Make a Correct Heap

當 root 沒有在 heap condition 時，如果分別由 root 的 2 個 child 做為 root 的 subheap 是符合 heap condition 的話，對 root 進行向下移動後就會形成包含 root 在內的正確 heap 結構。

所以我們可以從 array 末端開始對每個元素進行向下移動，每次做完就會得到 1 個正確的 heap，接著往前執行得到更高一層的 node 為 root 的 heap，以此類推直到對 index 0 的元素也就是 root 執行後便得到完整的 heap。

因為最底層的 leaf node 本身即可以視為是正確的 heap，所以可以不用對它們執行向下移動的步驟，因此我們可以從位於 N / 2 - 1 的 node 也就是高於 leaf node  1 層中最右邊的 node 開始，這樣只要執行 N  / 2 次即可。

