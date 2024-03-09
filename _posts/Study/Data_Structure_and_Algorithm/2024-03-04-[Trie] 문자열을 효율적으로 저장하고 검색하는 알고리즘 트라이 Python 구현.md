---
title: '[Trie] 문자열을 효율적으로 저장하고 검색하는 알고리즘 트라이 Python 구현'
author: baduk
date: 2024-03-05 20:51:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, python, problem solving, algorithm]
use_math: true
---

## 트라이 알고리즘이란?
트라이 알고리즘은 문자열을 빠르게 저장하고 찾도록 도와주는 알고리즘이다. 시간복잡도는 문자열의 길이 만큼의 선형적인 시간복잡도 O(N)을 보장한다.

## 트라이의 동작원리
### 트라이의 삽입과정
트라이는 문자열을 삽입할 때 문자열의 맨 앞 문자 부터 한 개씩 딕셔너리의 key로 받아 저장한다. 앞 문자를 key로 받았다면, 다시 그 앞 문자의 value의 key로 그 다음 문자를 받아 저장한다.

예를들어 ABC라는 문자열을 트라이에 저장한다고 해보자. 그럼 처음엔 ABC의 맨 앞 문자인 A를 딕셔너리의 key로 받고, 그 다음 루프에서 A의 value로서 B를 key로 받는다. 그리고 그 다음 루프에서는 B의 value로서 C를 key로 받아 저장한다.

결과적으로 트라이는 아래와 같이 저장될 것이다.
`{'A':{'B':{'C':{ } } } }`

또한 추가적으로 맨 끝의 문자는 'C'로 종결되었다는 정보를 담아야 한다. 그 이유는 문자 AB를 담은것도 아니고 문자 ABCD를 담은것도 아닌 ABC 이 딱 세글자 단어를 저장했다는 정보를 담아야 하기 때문이다. 따라서 문자 C에 대해서 is_end_of_word 라는 객체의 속성을 True로 변경해야 한다.

### 트라이의 검색과정
트라이의 검색과정도 삽입과정과 로직이 비슷하다. 찾고자 하는 문자열을 입력하여, 먼저 문자열의 맨 앞 문자를 가져와서 root노드로 부터 딕셔너리에 그 앞 글자가 있는지 확인한다. 만약 있다면 해당 문자를 key로 접근하여 다음 노드로 이동하고, 그 다음 노드의 딕셔너리에서 두번째 문자가 있는지 확인하고 세번째 문자도 이와 마찬가지로 다음노드 이동후 딕셔너리에 있는지 확인한다.

만약 찾고자 하는 문자의 길이가 3이라면 세 번째 문자의 탐색을 끝으로 종료를 하는데, 이때 is_end_of_word라는 객체의 속성이 False라면 해당문자가 끝이 아니었고 더 길게 있었던 다른 글자이므로, 즉 찾고자 하는 글자와 다르므로 False를 반환한다. 하지만 is_end_of_word가 True였다면 찾고자 하는 문자가 저장되어있었던 것이므로 True를 리턴하여 찾고자하는 문자가 트라이에 존재함을 출력한다.


## 트라이 파이썬 구현
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True

    def search(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end_of_word

# 테스트 코드
trie = Trie()
trie.insert("apple")
trie.insert("app")
print(trie.search("apple")) # True
print(trie.search("app")) # True
print(trie.search("ap")) # False
print(trie.search("appl")) # False
```

## .(점)을 통해서 문자열 검색하는 메소드 파이썬 구현

검색을 하려고 하는데, 완전한 글자가 잘 기억이 안나고 글자 수와 일부 글자만 기억이 나는 상황에서 검색하고 싶다면 어떻게 해야 할까?
이럴때는 기억안나는 부분을 .(점)으로 입력해서 검색하고 싶을 것이다. 이럴 때는 search메소드를 재귀적으로 작성하여 쉽게 구현할 수 있다. 로직은 간단하다. 앞서 위 코드에서 작성한 보통의 search 메소드와 달리 파라미터를 두 개를 받는다. 하나는 찾고자 하는 단어 word이고, 다른 하나는 노드의 주소다.

처음에 검색할 단어를 입력할 때는 노드를 root의 주소를 가리키도록 한다. 그리고 재귀적으로 호출 할 것이기 때문에, 호출 종결 지점에서의 조건은 word의 빈 스트링이 들어가 있을 때다. 재귀 호출을 할 때 앞글자를 제외한 나머지 글자를 노드의 주소와함께 파라미터로 넘길 것이다. 만약 파라미터로 받아온 word의 첫 글자가 .(점)이면 해당 노드가 가지고 있는 children의 모든 value를 대상으로 검색을 진행한다. 만약 검색을 하다가 하나라도 True가 나오면, 즉 검색하고자 했던 문자열이 한 개라도 존재하면, True를 반환한다. 하지만 모든 value를 대상으로 전체 확인을 했으나 없었다면 False를 반환한다.



```python
    def search(self, word: str, node=None) -> bool:
        if node == None:
            node = self.root

        if len(word) == 0:
            return node.is_end_of_word

        if word[0] == '.':
            for child in node.children.values():
                if self.search(word[1:], child):
                    return True
            return False

        if word[0] in node.children:
            return self.search(word[1:],node.children[word[0]])
```