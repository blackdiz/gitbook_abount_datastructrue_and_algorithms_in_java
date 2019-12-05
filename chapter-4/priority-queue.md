# Priority Queue

Priority queue內的元素是有序的, 比方由數字小到大的排序, 而最小的永遠在第一個位置，所以在插入元素時必須按順序放在正確的index上。

Priority queue可以用在許多情境上，例如可以在graph中找到minimum spanning tree\(最小生成樹\)，或者在多工系統中，待執行的程序可放在一個priority queue中，這樣有最高優先權的程序就可以被優先執行。

為了提高插入元素時的效率，priority queue經常使用heap實作，但此章節先使用array實作，因為它比較簡單。

#### 程式碼

```java
package com.blackdiz.algorithm.chapter4;

/**
 * 
 * @author blackdiz
 */
public class PriorityQueue {
  private int maxSize;
  private int[] queue;
  private int nItems = 0;

  public PriorityQueue(int max) {
    maxSize = max;
    queue = new int[max];
  }

  public void insert(int item) {
    if (isFull()) {
      System.out.println("Queue is full.");
      return;
    }
    if (nItems == 0) { // 如果目前queue內沒有元素則可以直接插入
      queue[nItems++] = item;
    } else {
      int i;
      for (i = nItems - 1; i >= 0; i--) { // 由後方開始比對
        int current = queue[i];
        // 如果新元素比目前這index的元素大, 則將目前index的元素往後移, 直到新元素比index的元素小或相同為止
        if (item > current) {
          queue[i + 1] = current;
        } else {
          break;
        }
      }
      queue[i + 1] = item; // 如果新元素比目前所指到index的元素小或相同就放在index的右邊
      nItems++;
    }
  }


  public int remove() {
    return queue[--nItems];
  }

  public boolean isFull() {
    return nItems == maxSize;
  }

  public boolean isEmpty() {
    return nItems == 0;
  }

  public void display() {
    System.out.println("---------------------------------");
    for (int i = 0; i < queue.length; i++) {
      System.out.print(queue[i] + ",");
    }
    System.out.println("");
  }

  public static void main(String[] args) {
    PriorityQueue q = new PriorityQueue(5);
    q.insert(10);
    q.display();
    q.insert(8);
    q.display();
    q.insert(9);
    q.display();
    q.insert(6);
    q.display();
    q.insert(7);
    q.display();

    System.out.println(q.remove());
    q.display();
    System.out.println(q.remove());
    q.display();
    System.out.println(q.remove());
    q.display();
    System.out.println(q.remove());
    q.display();
    System.out.println(q.remove());
    q.display();
  }
}
```

#### 時間複雜度

* 插入在最糟的情況下必須比較完queue內的所有元素，故複雜度為O\(N\)
* 刪除因為只須取出目前queue最末端的元素，因此複雜度為O\(1\)

