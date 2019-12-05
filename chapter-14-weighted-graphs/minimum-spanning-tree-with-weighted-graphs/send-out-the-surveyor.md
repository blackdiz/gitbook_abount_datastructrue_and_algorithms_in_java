# Send Out the Surveyor

因為建出 minimum spanning tree 的演算法有點複雜，所以我們繼續用鋪設電視線路的例子來說明，假設我們是老闆且有數個員工負責調查線路費用。

電腦演算法沒辦法一次處理問題相關的所有資料得到問題的最終答案，它必須一點一點地取得資料修正當下的答案。在 graph 的問題上，演算法會從某個 vertex 出發接著慢慢往外擴展取得當下最鄰近的 vertex 資料，然後再繼續向外擴展，如同 depth-first search 和 breadth-first search 所做的。

相同地，我們假設一開始並不知道所有城市間的線路費用為何，因此我們必須派出員工調查。

![](../../.gitbook/assets/city_cable_example.svg)

#### Starting in Ajo

我們從 Ajo 開始，只有 2 個城市 Bordo 和 Danza 可以到達，因此我們派出 2 名員工分別前往這 2 座城市調查從 Ajo 鋪設線路到達當地的費用。

第 1 名員工到達 Bordo 後回覆需要 6 百萬，而第 2 名員工則回報鋪設到 Danza 需要 4 百萬，我們把這些資料記錄下來：

* Ajo - Danza, 4 百萬
* Ajo -  Bordo, 6 百萬

#### Building the Ajo-Danza Link

這時我們可以決定選擇 Ajo - Danza 路線。原因是假設有其他路線比直接從 Ajo 到 Danza 便宜，那條路線一定要經過 Bordo 再繞回 Danza，但我們已知從 Ajo 到 Bordo 的費用較高，所以即使後續的路線費用很低，從 Bordo 回到 Danza 時一定會經過多餘的路線，因此費用會比直接到 Danza 高，因此我們此時先選擇最便宜的 Ajo - Danza 路線。

#### Building the Ajo - Bordo Link

接著我們由 Danza 可以到達鄰近的 Bordo、 Colina 和 Erizo，因此分別派出 3 名員工並得到費用分別是 7 百萬、8百萬和 1 千 2 百萬。同樣地我們將這些資訊記錄起來：

* Ajo - Bordo, 6 百萬
* Danza - Bordo, 7 百萬
* Danza - Colina, 8 百萬
* Danza - Erizo, 1 千 2 百萬

Ajo - Danza 路線因為已經鋪設所以從記錄裡刪除。

到這裡可以看出最好的選擇規則依據上述的推論就是每次都挑選清單中最便宜的路線。所以我們這次鋪設 Ajo 到 Bordo 的線路。

在此我們可以將城市分成 3 種類型：

1. 已連接線路的城市，在 graph 中它們就是已在 minimum spanning tree 中
2. 已知由任一目前已被連接的城市鋪設線路到當地的費用但尚未連接的城市
3. 目前完全沒有資料，未知的城市

在目前的階段中，Ajo、Danza 和 Bordo 屬於第 1 類，而 Colina 和 Erizo 屬於第 2 類，最後 Flor 屬於第 3 類，而整個過程就是將第 3 類的城市變成 第 2 類，第 2 類城市變成第 1 類。

#### Building the Bordo - Erizo Link

目前 Ajo、 Danza 和 Bordo 都已相連，而我們已知由 Danza 鋪設線路到第 2 類城市的費用，但還不曉得由 Bordo 到這些城市的費用，所以我們派出員工調查並得到：

* Bordo - Erizo, 7 百萬
* Danza - Colina, 8 百萬
* Bordo - Colina, 1 千萬
* Danza - Erizo, 1 千 2 百萬

在上個清單中的 Danza - Bordo 記錄已被刪除，因為我們不用考慮鋪設不管是直接或間接上已有線路連接的城市，而 Danza - Bordo 的線路已透過 Danza - Ajo - Bordo 的線路間接連接。

目前的清單中最便宜的選項是 Bordo - Erizo 的線路，因此我們這次鋪設 Bordo 到 Erizo 的線路。

#### Building the Erizo - Colina Link

從 Erizo 出發，我們得知 Erizo 到 Colina 和 Erizo 到 Flor 的鋪設費用：

* Erizo - Colina, 5 百萬
* Erizo - Flor, 7 百萬
* Danza - Colina, 8 百萬
* Bordo - Colina, 1 千萬

因此我們這次鋪設 Erio 到 Colina 的線路。

#### And, Finally, the Colina - Flor Link

當我們將清單中已連接線路的城市的鋪設路線刪除後，清單中剩下：

* Colina - Flor, 6 百萬
* Erizo - Flor, 7 百萬

所以我們最後鋪設 Colina 到 Flor 的線設，此時我們完成了所有工作，所有城市都有線路連接且是最便宜的鋪設方式。

