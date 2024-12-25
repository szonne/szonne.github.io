---
title: "Singly Linked List (단순연결리스트)"
categories:
  - data-structure
tags: "data-structure"
toc: true
toc_sticky: true
toc_label: "On This Page"
---

## 단순 연결 리스트 (Singly Linked List)

![단순연결리스트](/assets/images/singly-linked-list.png "Singly Linked List")


단순 연결 리스트는 동적 메모리 할당을 이용하여 노드들을 한 방향으로 연결하여 리스트를 구현하는 자료구조다.

단순 연결 리스트의 경우 단 방향으로 노드들이 연결되어 있기 때문에 삽입이나 삭제 시에 노드들의 이동이 필요하지 않다. 삽입과 삭제 시에는 기존 노드의 앞, 뒤 노드만 적절히 처리해주면 된다. 이 자료구조의 단점은 탐색 시에 항상 첫 노드부터 원하는 노드를 찾을 때까지 순차 탐색(Sequential Search)을 해야한다는 점이다.

### 구현 (Python)

연결리스트를 `Python`으로 구현하려면 필요한 기능들은 다음과 같다.
- 노드는 데이터와 다음 노드를 가리키는 포인터로 구성
  ```python
  class Node:
      def __init__(self, data, link):
      self.data = data
      self.next = link
  ```

- 연결 리스트 클래스 정의
  - 리스트의 크기를 출력하는 함수
  - 리스트가 비어있는지 확인하는 함수
  - 리스트의 맨 앞으로 노드를 추가하는 함수
  - 리스트의 특정 노드 뒤로 새 노드를 추가하는 함수
  - 리스트의 맨 앞 노드를 삭제하는 함수
  - 리스트의 특정 노드 뒤에 있는 노드를 삭제하는 함수
  - 순차 탐색하는 함수
  - 연결 리스트를 모두 출력하는 함수

  ```python
  class SList:
      def __init__(self):
          self._size = 0
          self.head = None
  
      @property
      def size(self):
          return self._size
  
      def is_empty(self):
          return self._size == 0
          
      def insert_at_front(self, data):
          if self.is_empty():
              self.head = Node(data, None)
          else:
              prev_link = self.head
              self.head = Node(data, prev_link)
              
          self._size += 1
      
      def insert_after(self, data, node):
          # 기존에 가리키던 node를 데이터 변수에 저장
          original_next_node = node.next
          
          # 새 노드 생성
          new_node = Node(data, original_next_node)
          
          # 새로 생성한 노드를 삽입
          link.next = new_node
          
          self._size += 1
          
      def delete_at_front(self):
          if self.is_empty():
              raise ValueError('Empty list')
          
          self.head = self.head.next
          
          self._size -= 1
      
      def delete_after(self, node):
          original_next_node = node.next
          
          link.next = original_next_node.next
          
          self._size -= 1
          
      def search(self, target):
          if self.is_empty():
              raise ValueError('Empty list')
          
          for i in range(self.size):
              if self.head.data == target:
                  return i
  
              self.head = self.head.next
          
          return -1
      
      def print_list(self):
          if self.is_empty():
              return None
          
          cur = self.head
          
          while cur:
              if cur.next:
                  print(cur.data, ' -> ', end='')
              else:
                  print(cur.data, end='')
                  
              cur = cur.next
  
  ```

- 간단한 예시 실행 결과
```python
  list_= SList()
  list_.insert_front('A')
  list_.insert_front('B')
  list_.insert_front('C')
  list_.print_list()
  
  # C  -> B  -> A
  
  print(f"B는 리스트의 {list_.search('B') + 1} 번째 위치함.")
  # B는 리스트의 2 번째 위치함.
  
  # 1을 B 다음에 추가
  list_.insert_after(1, list_.head.next)
  
  list_.print_list()
  # C  -> B  -> 1  -> A
  ```

이름 그대로 단순 연결 리스트의 특징은 데이터들이 서로 연결되어 있다는 점이다. 이러한 특성은 단순 연결 리스트가 여러 가지 장,단점을 갖는 이유가 된다.

### 장점
- 동적 메모리 관리
  - 최초 선언 시에 데이터 크기를 설정해야 하는 배열과 다르게, 사용할 때 필요에 맞춰 데이터 크기를 추가하거나 삭제할 수 있다.
  고정된 크기의 배열과 다르게 불필요하게 낭비되는 메모리를 줄일 수 있다.
- 데이터 추가 및 삭제 용이
  - 배열은 중간에 있는 특정 데이터를 삭제하거나 새로 추가하려면 기존의 다른 엘리먼트들을 모두 새롭게 이동시켜야 한다. 하지만, 단순 연결 리스트의 경우 데이터 삭제나 추가에 맞춰 포인터(연결되는 지점)만 적절하게 수정하면 된다.
  - 배열의 크기가 아주 클 경우, 데이터를 새로 추가하거나 삭제하는 작업이 비효율적이다. 예를 들어, 백만 개의 원소가 있는 배열에서 가장 처음에 새로운 데이터를 추가하려면 총 백만 개의 원소를 이동시켜야 한다. 그러나 단순 연결 리스트는 포인터만 처리해주면 데이터 조작이 훨씬 간단하다.

### 단점
- 불안정한 구조
  - 포인터 연결을 적절하게 하지 못하면 데이터 유실 또는 잘못된 자료 구조가 될 수 있다.
- 데이터 접근의 비효율성
  - 배열은 인덱스를 통해 데이터를 O(1) 시간 복잡도로 접근할 수 있지만, 단순 연결 리스트는 원하는 데이터가 나올 때까지 순차 탐색을 계속해야 한다. 시간 복잡도는 O(n)으로 배열에 비해 비효율적이다.
  역방향 탐색이 불가능하다.
- 포인터 정보 저장을 위한 메모리 오버헤드
  - 연결 리스트는 원소뿐만 아니라 포인터 정보도 같이 저장해야 하므로, 연결 리스트의 크기가 커질수록 메모리 오버헤드가 커진다.
