# Minimum Spanning Tree with Weighted Graphs

#### An Example: Cable TV in the Jungle

假設我們現在要在 6 座城市間鋪設電視線路，因此總共要有 5 條線路連接這 6 座城市，不同線路的費用不同，要如何找出成本最低的鋪設方式：

![edge &#x65C1;&#x7684;&#x6578;&#x5B57;&#x70BA;&#x92EA;&#x8A2D;&#x8CBB;&#x7528;&#xFF0C;vertex &#x65C1;&#x70BA;&#x57CE;&#x5E02;&#x540D;&#x7A31;](../../.gitbook/assets/city_cable_example.svg)

上圖為線路圖的 weighted graph，要找出成本最低的鋪設方式是計算出此 graph 的 minimum spanning tree，tree 會有 5 條連線 \(也就是 vertex 數 - 1\) 將 6 座城市相連且總成本會最低。

