# Links

在 **linked list** 中，每個元素內都有1個 **link** ，link 內會有指向下一個 link 的參照，以此類推串連下去直到最後一個 link 它的下個參照為 null，而 linked list 則有第一個 link，如:

Linked List            Link               Link            Link           Link

first link        -&gt;     data               data            data           data

                              next      -&gt;      next     -&gt;    next    -&gt;    next    -&gt;   null

#### Relationship, Not Position

在 array 中，每個元素都佔有一個位置，我們可以透過 index 直接存取。但在 list 中，我們只能透過每個元素的參照依序找到所要的元素。

