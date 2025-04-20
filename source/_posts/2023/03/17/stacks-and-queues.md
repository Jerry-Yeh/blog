---
title: Stacks and Queues
abbrlink: 62505
date: 2023-03-17 07:20:12
tags:
  - Cracking the Coding Interview
  - Stack
  - Queue
categories: Data Structure
thumbnail:
banner:
description:
---

<!-- @format -->

Stacks 與 Queues 是相對單純的資料結構，在取得資料時，只會依序取得最前或最後的那一筆資料，目的也是為了限制資料的使用; 而實作上，並不限於使用 Array 或 Linked List，只要符合個別的定義都可以稱為 Stacks 或 Queues。

<!-- more -->

## Stacks

採用後進先出 last-in-first-out (LIFO) 的方式排列，可以想像成蝶盤子的形式，最後疊上來的盤子會優先被取出，使用上常用於操昨工具時的上一步或瀏覽器的上一頁

![](stacks.png)

### Method

- pop: 刪除最上層的一筆資料，時間複雜度為 O(1)
- push: 加入一筆資料在最上層，時間複雜度為 O(1)
- peek: 取得最上層的一筆資料，但不刪除資料，時間複雜度為 O(1)
- isEmpty: 檢查 Stacks 是否為空

在 JavaScript 中，Array 有直接提供 method 來完成 Stack 的需求，而如果採用 Linked List 來實作，就需要使用 Doubly Linked List 來提供 previous data

```ts
export class Node {
  value: any;
  next: Node | null;
  prev: Node | null;

  constructor(value: any) {
    this.value = value;
    this.next = null;
    this.prev = null;
  }
}

export default class Stacks {
  length: number;
  head: Node | null;
  tail: Node | null;

  push(value: any) {
    const newNode = new Node(value);

    if (this.length === 0) {
      this.head = newNode;
      this.tail = newNode;
    } else {
      newNode.prev = this.tail;
      this.tail!.next = newNode;
      this.tail = newNode;
    }
    this.length++;
  }

  pop() {
    if (this.length === 0) {
      return null;
    } else if (this.length === 1) {
      const value = this.tail!.value;
      this.head = null;
      this.tail = null;

      return value;
    } else {
      const value = this.tail!.value;
      this.tail = this.tail!.prev;
      this.tail!.next = null;

      return value;
    }
  }

  peek() {
    if (this.length === 0) {
      return null;
    } else {
      return this.tail!.value;
    }
  }

  isEmpty() {
    return this.length === 0;
  }
}
```

## Queues

採用先進先出 first-in-first-out (FIFO) 的方式排列，可以想像成排隊的人潮，第一個排隊的人就可以優先進場

![](queues.png)

### Method

- add (enqueue): 加入一筆資料在最後面，時間複雜度為 O(1)
- remove (dequeue): 移除最前面的一筆資料，時間複雜度為 O(1)
- peek: 取得最前面的一筆資料，但不刪除資料，時間複雜度為 O(1)
- isEmpty: 檢查 Queues 是否為空

```ts
export class Node {
  value: any;
  next: Node | null;
  prev: Node | null;

  constructor(value: any) {
    this.value = value;
    this.next = null;
    this.prev = null;
  }
}

export default class Queues {
  length: number;
  head: Node | null;
  tail: Node | null;

  add(value: any) {
    const newNode = new Node(value);
    if (this.length === 0) {
      this.head = newNode;
      this.tail = newNode;
    } else {
      newNode.next = this.head;
      this.head!.prev = newNode;
      this.head = newNode;
    }
    this.length++;
  }

  remove() {
    if (this.length === 0) {
      return null;
    } else if (this.length === 1) {
      const value = this.head!.value;
      this.head = null;
      this.tail = null;

      return value;
    } else {
      const value = this.head!.value;
      this.head = this.head!.next;
      this.head!.prev = null;

      return value;
    }
  }

  peek() {
    if (this.length === 0) {
      return null;
    } else {
      return this.head!.value;
    }
  }

  isEmpty() {
    return this.length === 0;
  }
}
```

## Array 與 Linked List 比較

在 JavaScript 中，如果使用 Array 來實作 Stacks 與 Queues，其實都有預設的 Array functions 可以直接達到目的，只是因為 Queues 所使用的 remove (`unshift`) 會改變所有 Array 的 index，所以在效能上會比較差 (同理 `shift` 也是)，所以如果有效能上的考量，Queues 會建議使用 Linked List 來進行實作。

## 資料參考

[Cracking the Coding Interview](https://crackingthecodinginterview.com/)
[[資料結構] Stacks and Queues](https://pjchender.dev/dsa/dsa-stacks-queues/)
[]()
