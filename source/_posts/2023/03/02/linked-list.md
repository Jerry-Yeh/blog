---
title: Linked List
abbrlink: 18120
date: 2023-03-02 21:42:38
tags:
  - Cracking the Coding Interview
  - Linked List
categories: Data Structure
thumbnail:
banner:
description:
---

<!-- @format -->

Linked List 是一種包含順序的資料結構，但和 Array 不同的是，沒有 index 來指出特定 Node，因此，如果要尋找 list 中的某個 Node，就需要遍歷整個 list; 因大多數程式語言在宣告 Array 時，都會固定其長度，並在記憶體中做連續性的儲存，Linked List 就是為了解決這種長度固定的問題而存在，避免太小不夠用或太大浪費空間。

為了達到彈性長度的目的，在記憶體中儲存時就會不連續，所以每個 Node 都會明確指出下一個 Node，以確保資料之間的鏈結。

<!-- more -->

而 Listed List 又分成三種，分別是 Singly、Doubly Linked List & Circularly Linked List，所有的 Node 都包含 value 與下一個 Node 的資訊 next，Doubly & Circularly Linked List 的 Node 還會額外包含上一個 Node 的資訊 prev

## Singly Linked List

![](singly-linked-list.png)

- 每個 Node 會包含 value
- 每個 Node 會包含下一個 Node 的指標
- 從頭部 head 開始，依序往下讀取至尾部 tail 結束

## Doubly Linked List

![](doubly-linked-list.png)

- 每個 Node 會包含 value
- 每個 Node 會包含上一個與下一個 Node 的指標
- 從頭部 head 開始，依序往下讀取至尾部 tail 結束
- 因具備 prev 指標，所以也可以從 tail 開始往前尋找

## Circularly-Linked-List

![](circularly-linked-list.png)

- 每個 Node 會包含 value
- 每個 Node 會包含上一個與下一個 Node 的指標
- 從頭部 head 開始，依序往下讀取至尾部 tail，頭尾指標彼此會再接在一起，形成一個循環
- head prev 會指向 tail，tail next 會指向 head

## JavaScript 實作

在 JavaScript 中，並沒有內建的 Linked List，所以必須透過 Object 來實作，其中包含節點 Node、Linked List 與其所提供的方法 methods

### Node

Node 會包含其所代表的資料與指標

- `value`: 資料
- `next`: 指向下一個 Node 的指標
- `prev`: 指向上一個 Node 的指標(Singly Linked List 並未包含)

```ts
class Node {
  value: any;
  next: Node | null;
  prev: Node | null; // Doubly only

  constructor(value: any) {
    this.value = value;
    this.next = null;
    this.prev = null;
  }
}
```

### Linked List

Linked List 會提供幾項基本屬性與方法供使用者進行操作

#### 屬性 Property

- `length`: Linked List 的長度
- `head`: 指向第一個 Node，如果 `length` 為 0，則為 null;如果 `length` 為 1 則和 `tail` 指向同一個 Node
- `tail`: 指向最後一個 Node，如果 `length` 為 0，則為 null;如果 `length` 為 1 則和 `head` 指向同一個 Node

```ts
class LinkedList {
  length: number;
  head: Node | null;
  tail: Node | null;

  constructor() {
    this.length = 0;
    this.head = null;
    this.tail = null;
  }
}
```

#### 方法 Method

- `append`: 在 `tail` 插入一個 Node

  ```ts
  append(value: any) {
    const newNode = new Node(value);
    if (this.length === 0) {
      // Linked List is empty
      this.head = newNode;
      this.tail = newNode;
    } else {
      newNode.prev = this.tail;  // Doubly only
      this.tail!.next = newNode;
      this.tail = newNode;
    }
    this.length++;
  }
  ```

- `preppend`: 在 `head` 插入一個 Node

  ```ts
  preppend(value: any) {
    const newNode = new Node(value);
    if (this.length === 0) {
      // Linked List is empty
      this.head = newNode;
      this.tail = newNode;
    } else {
      newNode.next = this.head;
      this.head!.prev = newNode;  // Doubly only
      this.head = newNode;
    }
    this.length++;
  }
  ```

- `getNode`: 因為 Linked List 不像 Array 可以直接透過 index 到達指定位置，所以需先根據 index 遍歷取得指定位置

  ```ts
  getNode(index: number) {
    if (index < 0 || index > this.length) {
      return null;
    }

    // search Node from head
    let tempNode = this.head;
    let tempIdx = 0;
    while (tempIdx < index) {
      tempNode = tempNode!.next;
      tempIdx++;
    }

    return tempNode;
  }
  ```

- `insert`: 於指定位置插入一個 Node

  ```ts
  insert(index: number, value: any) {
    if (index >= this.length) {
      this.append(value);
      return;
    }

    const newNode = new Node(value);

    if (index <= 0) {
      newNode.next = this.head;
      this.head = newNode;
    } else {
      const prevNode = this.getNode(index - 1) as Node;
      const currNode = prevNode.next as Node;
      prevNode.next = newNode;
      newNode.prev = prevNode; // Doubly only
      newNode.next = currNode;
      currNode.prev = newNode; // Doubly only
    }
    this.length++;
  }
  ```

- `remove`: 刪除指定 Node

  ```ts
  remove(index: number) {
    if (index < 0 || index >= this.length || this.length === 0) {
      return;
    } else if (index === 0) {
      this.head = this.head!.next;
      this.head!.prev = null;
    } else {
      const prevNode = this.getNode(index - 1) as Node;
      const nextNode = this.getNode(index + 1);

      prevNode.next = nextNode;

      if (!nextNode) {
        this.tail = prevNode;
      } else {
        nextNode.prev = prevNode;
      }
    }
    this.length--;
  }
  ```

- `print`: 按照 Linked List 印出所有 Node value

  ```ts
  print(): any[] {
    let array: any[] = [];
    let tempNode = this.head;

    while() {}

    return array;
  }
  ```

## 時間複雜度

在取得節點的情況下，插入與刪除為 O(1)，但如果沒有取得節點就會是 O(n)，而因為不如 Array 有明確的 index，所以搜尋為 O(n)

- 插入 insert、刪除 delete: O(1) or O(n)
- 新增/刪除頭部 prepend、新增/刪除尾部 append: O(1)
- 存取 & 搜尋 lookup: O(n)

## 參考資料

[Cracking the Coding Interview](https://crackingthecodinginterview.com/)
[[資料結構] Array and Linked List](https://pjchender.dev/dsa/dsa-array-linked-list/)
[Linked List Wiki](https://en.wikipedia.org/wiki/Linked_list)
[JavaScript 學演算法（五）- 鏈結串列 Linked list](https://chupai.github.io/posts/200427_ds_linkedlist/)
