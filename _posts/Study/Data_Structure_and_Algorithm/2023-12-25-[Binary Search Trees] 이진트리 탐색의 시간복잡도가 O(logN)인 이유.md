---
title: '[Binary Search Trees] 이진트리 탐색의 시간복잡도가 O(logN)인 이유'
author: baduk
date: 2023-12-25 14:27:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, Binary Search tree, 이진트리, python, problem solving, algorithm]
use_math: true
---
이진트리 탐색에서 시간복잡도가 일반적으로 O(logN)인 이유를 설명해보겠다.

자료구조에서는 노드의 개수가 많으면 많을 수록, 작업시간이 더 오래 걸리게 되는 것은 당연한데. 일반적으로 이진트리는 아래 그림과 같이 생겼다. 아래 그림을 보면 노드의 개수가 총 7개인데, 아래 노드를 탐색하는 과정은 최대 딱 두 번만 거치게 된다. 즉, worst case가 최대 2이다. 왜냐하면 높이(h)가 2이기 때문이다.

![](https://lh3.googleusercontent.com/pw/ABLVV86lF9NNP7nb7JVe08eSDa3i3wDInvpHsw8VdqbAPkNmXIt5oeFjzVhPlQePDeGONbpdjyWLs_nrJwc_w1BGDd_s0L8sVzBwYpVp5KSVbf66q1_nrRp8rlXJPIjuZsWrf_NWA2SHUMFYDUuaPEMjJJ4=w494-h488-s-no-gm?authuser=0)

만약 연결리스트 였으면 어땠을까? 연결리스트는 아래 그림과 같이 일자로 7개의 노드가 쭉 연결되어 있기 때문에, 탐색하는 과정에서 최악의 경우 최대 노드의 개수만큼 탐색 작업을 거쳐야 할 것이다. 그래서 연결리스트의 탐색 시간 복잡도는 O(N)이다.

![](https://lh3.googleusercontent.com/pw/ABLVV86be6kcEYJHzFv8tHUwYxisFK8ayiBJGb7WLZ3NX0NJbRaHs3gNDRHclxY0w8JOu92erdlzWXO89d0mjrYt-4HXEXFK-d6jlwW4diaRaHbg10vqt-zDg8NNNddJoP4Im8_MBS47VNtTgku9FnXsEC8=w1914-h860-s-no-gm?authuser=0)

그렇다면 이진트리에서 탐색 시간복잡도가 O(logN)이 될 수 있는 이유는 무엇일까? 일단 먼저 한번 쉽게 생각해보자. 이진트리에서 노드의 개수가 한개 늘어난다고 해서 높이 자체가 무조건 정비례해서 늘어나지 않는다. 즉, 다르게 말하자면 노드의 개수가 한개 늘어난다고해서 탐색작업이 무조건 한 개 더 늘어나는 것은 아니다. 예를들면 아래 그림과 같이 노드가 8개인 이진트리에 노드가 3개 더 늘어나서 노드의 총 개수가 11개가 되었다고 해도 높이(h)는 전과 후 둘다 3으로 동일하다.

![](https://lh3.googleusercontent.com/pw/ABLVV86MD22FbeW3m-HxP8X_4j0f5_-KUxYogvBl5fpzIIsd7wVN3UE3G1bSL9St3rVr0rmlcpmC4jDqZwhZADEL_uz66RUf8aUqmid8vTWo0m3Zg_jHdmE5JLKkNJaNwRYZG_-VGT8D8bGYLdrLIP4QUjs=w1984-h1114-s-no-gm?authuser=0)

즉, 이 상황에서는 노드의 개수가 늘어났다고해서 시간복잡도의 영향을 주지 않는다.(단, 차일드노드가 왼쪽 또는 오른쪽으로만 붙어서 늘어나면 시간복잡도에 직접적으로 영향을 줄 수 있다. 여기서는 차일드 노드가 왼쪽 오른쪽 골고루 붙었을 때를 말하는 것이다.) 따라서 이진트리에서 시간복잡도는 O(N)보다는 무조건 작다고 할 수 있다.

이제 이진트리의 시간복잡도가 왜 수치적으로 logN인지 수식으로 한번 접근해보자. 노드의 개수(n)는 아래 그림과 같이 표현될 수 있다. 그리고 여기서 h는 높이를 뜻하는데, 이 h가 바로 최악의 경우의 시간복잡도라고 생각하면 된다. 즉 이진트리에서 최악의 경우에서의 시간복잡도는 높이(h)보다 클 수 없다.

![](https://lh3.googleusercontent.com/pw/ABLVV85qq8ShX64WswJUcRJ2MCkafeuGrUavrrwh_BrxTc97MLEOz-hu7-uQxyj6gUGpAnu9xcipn-ixheCldm_JLTNHHGDJV4P5KTJ8OI1EdTlKxMjN5SGrIEYLn6AlBlVb-jJfhUXdSix7H6qFudQMJpo=w1986-h1084-s-no-gm?authuser=0)

그렇다면 이 h를 구하면 시간복잡도를 구할 수 있게 될 것이다. 위 그림과 같은 식의 과정을 거치면 

$h=\log_2 (n+1) - 1$
되는데, 여기서 빅오표현를 위해서 군더더기를 제거해주면 최종적으로 $h=\log n$ = O(log n)이 된다. 하지만 이진트리의 시간복잡도가 항상 O(log n)인 것은 아니다. 그 이유는 아래 그림과 같이 이진트리가 연결리스트스럽게 일자로 쭉 늘어설 경우 시간복잡도가 O(N)으로 나오기 때문이다.

![](https://lh3.googleusercontent.com/pw/ABLVV87C6_5_IXToFM7RFUjgGiYWWTE0sox3-dJsYW7t1l5ur5PZ3xPqE4ko4uOfSSLtUmNxhV_4VpsPlsOacvmCrxF9bUC_WC29947o6rheiyjntRUxVtqiJXWyPe6NFd7QdsJzBYs1xhNf0VZ-D9W5G-g=w1330-h1422-s-no-gm?authuser=0)

하지만 게시글 맨 위 사진처럼 각각의 노드가 균형을 갖춘 일반적인 모양의 이진트리의 시간복잡도는 O(logN)이다.

이상 이진트리의 시간복잡도에 관하여 설명하였다.