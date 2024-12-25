---
title: "Doubly Linked List (이중연결리스트)"
categories:
    - data-structure
tags: "data-structure"
toc: true
toc_sticky: true
toc_label: "On This Page"
---

## 이중 연결 리스트 (Doubly Linked List)

![이중연결리스트](/assets/images/doubly-linked-list.png "Doubly Linked List")

이중 연결 리스트는 단순 연결 리스트가 갖고 있는 단점을 보완한다.
단순 연결 리스트가 단방향으로 노드들이 연결되어 있는 것에 반해, 이중 연결 리스트는 이름 그대로 노드들이 양방향으로 연결되어 있다.

### 구현 (Python)
- 노드는 단순 연결 리스트와 다르게, 다음 노드와 이전 노드를 가리키는 레퍼런스를 알아야 한다. 
```python
class Node:
    def __init__(self, data, prev, next):
        self.data = data
        self.prev = prev
        self.next = next
```