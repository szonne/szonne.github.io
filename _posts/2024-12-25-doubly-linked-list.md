---
title: "Doubly Linked List (이중연결리스트)"
categories:
    - data-structure
tags: "data-structure"
toc: true
toc_sticky: true
toc_label: "On This Page"
related: "/_posts/2024-12-22-singly-linked-list.md"
---

## 이중 연결 리스트 (Doubly Linked List)

![이중연결리스트](/assets/images/doubly-linked-list.png "Doubly Linked List")

이중 연결 리스트는 단순 연결 리스트가 갖고 있는 단점을 보완한다.
단순 연결 리스트가 단방향으로 노드들이 연결되어 있는 것에 반해, 이중 연결 리스트는 이름 그대로 노드들이 양방향으로 연결되어 있다.

### 구현 (Python)
- 노드는 단순 연결 리스트와 다르게, 다음 노드와 이전 노드를 가리키는 레퍼런스를 알아야 한다. 
```python
class Node:
    def __init__(self, data, prev, next_):
        self.data = data
        self.prev = prev
        self.next = next_
```

#### 이중 연결 리스트의 생성자 구현
  - 최초 초기화 시에 이중 연결 리스트의 크기는 `0`
  - 단순 연결 리스트와 다르게 이중 연결 리스트에서는 head와 tail 2개의 포인터가 있고, head는 리스트의 가장 처음, tail은 리스트의 가장 마지막 노드를 가리킨다.
  - 2개의 포인터가 있음으로써 리스트의 앞단, 혹은 끝단에서의 탐색이나 데이터 삭제, 추가 등이 용이해질 수 있다.

  ```python
  class DList:
      def __init__(self):
          self._size = 0
          self.head = Node(
              data=None,
              prev=None,
              next_=None,
          )
          self.tail = Node(
              data=None,
              prev=self.head,
              next_=None
          )
          self.head.next = self.tail
  ```

#### 이중 연결 리스트의 유틸 메소드 구현.
  ```python
  class DList:
      ...
      @property
      def size(self):
          return self._size
      
      def is_empty(self):
          return self.size == 0
  ```

#### 이중 연결 리스트의 데이터 삽입 관련 메소드 구현
  - 단순 연결 리스트와의 차이점은 새로운 노드를 추가할 때 이전, 이후 노드를 모두 수정해줘야한다는 점이다.
  - `A <-> B` 사이에 `C` 노드를 추가한다고 하면, A의 next를 B에서 C로 수정하고 B의 prev를 A에서 C로 수정해야 한다. 
  ```python
  class DList:
      ...
      def insert_before(self, node, data):
          prev_node = node.prev
          new_node = Node(data=data, prev=prev_node, next_=node)
          
          prev_node.next = new_node
          node.prev = new_node
          
          self._size += 1
          
      def insert_after(self, node, data):
          next_node = node.next
          new_node = Node(data=data, prev=node, next_=next_node)
          
          node.next = new_node
          next_node.prev = new_node
          
          self._size += 1
  ```

#### 이중 연결 리스트의 삭제 관련 메소드 구현
  - 삭제하려는 노드의 앞, 뒤에 위치한 노드를 서로 이어준다.
  - 삭제하려는 대상이 head, tail일 경우에 대한 예외처리를 추가한다.
  ```python
  class DList:
      ...
      def delete(self, node):
          # 삭제하려는 노드가 head인 경우
          if not node.prev:
              self.head = node.next
          else:
              node.prev.next = node.next
              
          # 삭제하려는 노드가 tail인 경우
          if not node.next:
              self.tail = node.prev
          else:
              node.next.prev = node.prev
          
          self._size -= 1
  ```

#### 이중 연결 리스트의 아이템 출력 관련 메소드 구현
  - `head`, `tail`을 이용해서 순차적으로 혹은 역순으로 리스트 아이템을 출력할 수 있다.
  - `reverse` 인자를 통해서 앞에서부터 혹은 뒤에서부터 출력할 지 결정한다. 
  ```python
  class DList:
      ...
      def print_list(self, reverse=False):
          if self.is_empty():
              print('')
              return
          
          if not reverse:
              curr = self.head.next
              result = f'{curr.data}'
              
              while curr.next != self.tail:
                  result += f'->{curr.next.data}'
                  curr = curr.next
          else:
              curr = self.tail.prev
              result = f'{curr.data}'
              while curr.prev != self.head:
                  result = f'{curr.prev.data}<-' + result
                  curr = curr.prev
              
          print(result)
  ```

#### 완성된 이중 연결 리스트 클래스를 활용한 예시 코드
  ```python
  d_list = DList()
  d_list.insert_after(d_list.head, 'A')
  d_list.insert_after(d_list.head, 'B')
  d_list.insert_after(d_list.head, 'C')
  
  d_list.insert_after(d_list.head.next, 1)
  d_list.print_list(reverse=False)
  # C->1->B->A
  
  d_list.delete(d_list.tail.prev)
  d_list.print_list(reverse=True)
  # C<-1<-B
  ```

#### 최종 코드
  ```python
  class Node:
      def __init__(self, data, prev, next_):
          self.data = data
          self.prev = prev
          self.next = next_
  
  class DList:
      def __init__(self):
          self._size = 0
          self.head = Node(
              data=None,
              prev=None,
              next_=None,
          )
          self.tail = Node(
              data=None,
              prev=self.head,
              next_=None
          )
          self.head.next = self.tail
          
      @property
      def size(self):
          return self._size
      
      def is_empty(self):
          return self.size == 0
      
      def insert_before(self, node, data):
          prev_node = node.prev
          new_node = Node(data=data, prev=prev_node, next_=node)
          
          prev_node.next = new_node
          node.prev = new_node
          
          self._size += 1
          
      def insert_after(self, node, data):
          next_node = node.next
          new_node = Node(data=data, prev=node, next_=next_node)
          
          node.next = new_node
          next_node.prev = new_node
          
          self._size += 1
          
      def delete(self, node):
          # 삭제하려는 노드가 head인 경우
          if not node.prev:
              self.head = node.next
          else:
              node.prev.next = node.next
              
          # 삭제하려는 노드가 tail인 경우
          if not node.next:
              self.tail = node.prev
          else:
              node.next.prev = node.prev
          
          self._size -= 1
          
      def print_list(self, reverse=False):
          if self.is_empty():
              print('')
              return
          
          if not reverse:
              curr = self.head.next
              result = f'{curr.data}'
              
              while curr.next != self.tail:
                  result += f'->{curr.next.data}'
                  curr = curr.next
          else:
              curr = self.tail.prev
              result = f'{curr.data}'
              while curr.prev != self.head:
                  result = f'{curr.prev.data}<-' + result
                  curr = curr.prev
              
          print(result)
  ```

### 장점
- 양방향 탐색 가능
  - 이중 연결 리스트의 대표적인 장점으로 양방향 탐색을 꼽을 수 있다. 단순 연결 리스트는 단방향의 연결로 인해서 역순으로 탐색하는 것이 구조적으로 불가능하다. 하지만 이중 연결 리스트의 경우 포인터가 2개(`head`, `tail`) 있기 때문에 앞에서부터 혹은 뒤에서부터 리스트 탐색이 가능하다.
  - 하지만, 시간 복잡도 자체는 `O(n)`으로 단순 연결 리스트와 동일하다. 
- 삭제, 삽입의 편리성
  - 단순 연결 리스트와 비교했을 때 특정 노드를 찾기가 더 쉽기 때문에 결과적으로 노드 삽입이나 삭제가 더 편리해진다는 장점도 있다.

### 단점
- 메모리의 추가 사용
  - 단순 연결 리스트와 비교했을 때, 포인터가 2개로 늘어나면서 메모리 사용량이 조금 더 많다.
- 구조의 복잡성
  - 단순 연결 리스트의 간단한 구조에 비해서 약간 더 복잡한 구조로 되어있다.

### 사용처
- 이중 연결 리스트는 양방향 탐색과 삽입 또는 삭제의 효율성이 필요한 환경에서 유용할 수 있다.
특히 **브라우저 기록**, 캐시 관리, 작업 스케줄링, 파일 시스템 같은 시스템 프로그래밍이나 텍스트 편집기, 데이터 처리 알고리즘과 같은 응용 프로그램에서 자주 활용된다.
- 브라우저 방문 기록에서 뒤로, 혹은 앞으로 탐색하는 경우가 많다. 이전 페이지나 다음 페이지로 이동할 때 이중 연결 리스트 구조가 사용된다.
- LRU(Least Recently Used) Cache에서도 이중 연결 리스트가 활용된다. 캐시에 있는 데이터를 참조할 때, 노드를 앞으로 이동하거나 제거할 때 이중 연결 리스트 구조가 사용된다. 