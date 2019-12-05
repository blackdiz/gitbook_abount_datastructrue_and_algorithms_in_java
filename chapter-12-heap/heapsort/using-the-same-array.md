# Using the Same Array

heapsort 最簡單的做法是將未排序的 array 中的元素一一新增到 heap 中再一一取出得到排序後的元素，這樣我們需要 2 個長度為 N 的 array，一個是未排序的 array，另一個是 heap array。但我們可以使用同一個 array 進行，這樣可以減少一半的記憶體花費。

當元素新增到 heap 中後，我們對其中一半的元素執行向下移動的步驟以得到正確的 heap 結構，這個過程只需要 heap array，因此此時不需要額外的 array。

但在刪除時，取出的元素如果沒有額外的 array 要儲存在何處 ? 每次當元素從 heap 中刪除時，heap array 的最末端就會空出，而 heap 的大小就會變小，因此我們可以將取出的元素儲存在 heap array 的最末端。隨著元素一一被取出，heap array 的長度慢慢變短，而儲存已排序元素的 array 則慢慢變長，這樣我們就不需要額外的 array。

