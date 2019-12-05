# Dijkstra's Algorithm

Edsger Dijkstra 在 1959 年發明 Dijkstar 演算法，這個演算法不只可以找出由某 vertex 到另一 vertex 的最短路徑，更可以找出由某 vertex 到所有其他 vertex 的最短路徑。

#### Agents and Train Rides

接下來應用 Dijkstra 演算法來找出由 Ajo 到其他城市最便宜的路線。我們雇用了一些人來調查每站到其他站的票價為何，但站長只能給我們該站直達其他站的票價，而我們會把這些資料都記錄下來。

#### The First Agent: In Ajo

我們對每一站都派一個人去調查票價，在 Ajo 這裡我們得知到 Bordo 花費 50 元，而到 Danza 是 80 元，並且將資訊記錄下面這份最低票價表：

| 從 Ajo 到 | Bordo | Colina | Danza | Erizo |
| :--- | :--- | :--- | :--- | :--- |
| 步驟 1 | 50 \(由 Ajo\) |  | 80 \(由 Ajo\) |  |

接下來我們要遵循這條規則：下一個站是加上當前所有票價後最便宜的路線，這和之前 minimum spanning trees 不同，那時是選擇連接到尚未被連接的目標路線中最便宜的，但這次我們是要將之前已選擇的票價總和跟尚未選取的路線加總後再選擇最便宜的選項。

#### The Second Agent: In Bordo

接下來我們知道由 Bordo 到 Colina 是 60 元，而到 Danza 是 90 元。在加上由 Ajo 到 Bordo 的 50 元後，等於是由 Ajo 經 Bordo 到 Colina 是 110 元，由 Ajo 經 Bordo 到 Danza 是 140 元，這裡的重點是因為我們已知由 Ajo 直達 Danza 只需要 80 元，因此我們在表格中只記錄較便宜的選項，所以最終票價表修改成：

| 從 Ajo 到 | Bordo | Colina | Danza | Erizo |
| :--- | :--- | :--- | :--- | :--- |
| 步驟 1 | 50 \(由 Ajo\) |  | 80 \(由 Ajo\) |  |
| 步驟 2 | 50 \(由 Ajo\)\* | 110 \(經 Bordo\) | 80 \(由 Ajo\) |  |

\* 表示目前已確定為最低票價路線

這時我們可以知道目前的路線是最便宜的，因為如果有比 Ajo 直達 Bordo 更便宜的路線則勢必會經過其他站，除了 Bordo 外唯一的站就只有 Danza，但我們已知由 Ajo 到 Danza 比到 Bordo 貴，因此不論之後的路線票價多少都一定比 Ajo 直達 Bordo 的路線貴。

#### Three Kinds of Towns

這裡我們可以將這些車站分成 3 種類型：

1. 我們已經確定在路線中的站
2. 我們已知票價但還未選入路線中的站
3. 未知的站

目前 Ajo 和 Bordo 屬於第 1 類，第 1 類的站會形成一個 tree，包含由起始 vertex 開始到不同 vertex 的 path。Colina 和 Danza 則為第 2 類，我們已知由已在路線中的站到它們的票價為何。最後 Erizo 則是第 3 類，我們完全不知道到該站的票價資訊。

如同 minimum spanning tree 一樣，演算法也是將第 3 類轉為第 2 類，第 2 類轉為第 1 類的過程。

#### The Third Agent: In Danza

目前我們已知但尚未排進路線中的最便宜路線是由 Ajo 直達 Danza 的 80 元，因此我們將 Danza 排入路線中，同時我們知道由 Danza 到 Colina 是 20 元，而 Danza 到 Erizo 為 70 元。相較由 Ajo - Bordo - Colina 的 110 元，由 Ajo 到 Danza 為 80 元加上 Danza 到 Colina 的 20 元等於 Ajo - Danza - Colina 是 100 元，比原先的路線便宜，所以我們可以再修改我們的最低票價表：

| 從 Ajo 到 | Bordo | Colina | Danza | Erizo |
| :--- | :--- | :--- | :--- | :--- |
| 步驟 1 | 50 \(由 Ajo\) |  | 80 \(由 Ajo\) |  |
| 步驟 2 | 50 \(由 Ajo\)\* | 110 \(經 Bordo\) | 80 \(由 Ajo\) |  |
| 步驟 3 | 50 \(由 Ajo\)\* | 100 \(經 Danza\) | 80 \(由 Ajo\)\* | 150 \(經 Danza\) |

\* 表示目前已確定為最低票價路線

#### The Fourth Agent: In Colina

現在由 Ajo 到 Colina 的最便宜路線是經由 Danza 的 100 元，所以我們把它排入最終路線中並派入到 Colina 調查票價。由 Colina 到 Erizo 要 40 元，因為 Ajo - Danza - Colina 總共為 110 元，加上 Colina - Erizo 的 40 元，所以 Ajo - Danza - Colina - Erizo 為 150 元，這時我們再修改最低票價表：

| 從 Ajo 到 | Bordo | Colina | Danza | Erizo |
| :--- | :--- | :--- | :--- | :--- |
| 步驟 1 | 50 \(由 Ajo\) |  | 80 \(由 Ajo\) |  |
| 步驟 2 | 50 \(由 Ajo\)\* | 110 \(經 Bordo\) | 80 \(由 Ajo\) |  |
| 步驟 3 | 50 \(由 Ajo\)\* | 100 \(經 Danza\) | 80 \(由 Ajo\)\* | 150 \(經 Danza\) |
| 步驟 4 | 50 \(由 Ajo\)\* | 100 \(經 Danza\)\* | 80 \(由 Ajo\)\* | 140 \(經 Colina\) |

\* 表示目前已確定為最低票價路線

#### The Last Agent: In Erizo

這時尚未加入最終路線的站只剩 Erizo，所以我們將它加入並派入前往調查票價，結果由 Erizo 可到的站只有 Bordo，但 Ajo - Bordo 的路線已經加入最終路線了，所以我們就完成了最低票價表：

| 從 Ajo 到 | Bordo | Colina | Danza | Erizo |
| :--- | :--- | :--- | :--- | :--- |
| 步驟 1 | 50 \(由 Ajo\) |  | 80 \(由 Ajo\) |  |
| 步驟 2 | 50 \(由 Ajo\)\* | 110 \(經 Bordo\) | 80 \(由 Ajo\) |  |
| 步驟 3 | 50 \(由 Ajo\)\* | 100 \(經 Danza\) | 80 \(由 Ajo\)\* | 150 \(經 Danza\) |
| 步驟 4 | 50 \(由 Ajo\)\* | 100 \(經 Danza\)\* | 80 \(由 Ajo\)\* | 140 \(經 Colina\) |
| 步驟 5 | 50 \(由 Ajo\)\* | 100 \(經 Danza\)\* | 80 \(由 Ajo\)\* | 140 \(經 Colina\)\* |

\* 表示目前已確定為最低票價路線

這時我們已經得到由 Ajo 到其他任一站的最低票價的路線為何，這個過程其實就是 Dijkstra 演算法，過程中有幾個重點：

* 當我們得到新的票價資訊時需要更新目前的記錄，並且只保留最便宜的路線
* 當決定下一個目標站時選擇由起始站開始最便宜的票價路線

