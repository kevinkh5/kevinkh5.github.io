---
title: '[Tree] 트리의 종류(Full, Perfect, Complete)'
author: baduk
date: 2023-12-24 13:30:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, tree, python, problem solving, algorithm]
---
트리는 기본적으로 3가지 종류가 있다.

```
1. Full Tree
2. Perfect Tree
3. Complete Tree
```

### 1. Full Tree
Full 트리는 부모노드가 자식노드 두개를 다 가지고 있는 트리라고 생각하면 된다. 모든 노드의 Level이 달라도 자식노드 두개만 다 가지고 있으면 Full 트리라고 할 수 있다. 하지만 만약 부모노드가 한개의 자식노드만 가지고 있게 된다면, 더 이상 Full트리가 아니게 된다. 


### 2. Perfect Tree
Perfect 트리는 부모노드가 자식노드 두개를 다 가지고 있으면서, 모든 노드의 Level까지 동등해야한다.


### 3. Complete Tree
Complete 트리는 마지막 Level을 제외한 모든 서브트리의 레벨이 같아야 하며, 노드가 왼쪽부터 채워져 있어야 한다. 왼쪽부터 채워져 있으면 자식노드가 하나만 있어도 Complete Tree라고 할 수 있다. 하지만 자식노드가 오른쪽으로 1개만 연결되어있다면 그것은 더 이상 Complete Tree가 아니게 된다. 