# Not Really Swapped

當往上或往下移動時，我們用交換 node 位置的方式，但每次交換需要 3 次的複製：

```text
Node temp = a;
Node a = b;
Node b = temp;
```

假設有 A、B、C、D 4 個元素，要將 A 移動到 D 的位置需要交換 3 次位置也就要 9 次複製。我們可以將暫存 node 的步驟減少到 1 次來減少複製的次數：

```text
Node temp = a;
Node a = b;
Node b = c;
Node c = d;
Node d = temp;
```

這樣把 A 移動到 D 的位置只需要 5 次複製，當移動跨越多層時這兩種方法的差距就越大。

