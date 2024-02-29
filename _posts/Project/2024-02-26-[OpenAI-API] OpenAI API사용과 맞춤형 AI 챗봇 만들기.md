---
title: '[OpenAI-API] OpenAI API사용과 맞춤형 AI 챗봇 만들기'
author: baduk
date: 2024-02-26 19:44:00 +0900
categories: [Project, Toy Project]
tags: [OpenAi]
---
## API-KEY 등록하기

```python
import os
export OPENAI_API_KEY='your-api-key-here'
os.environ["OPENAI_API_KEY"] = "your-api-key-here"
```
<https://platform.openai.com/docs>
OpenAI 공식홈페이지에 들어가서 API-KEY를 받아 위 코드에 입력한다.

## API 사용 1 - Curl로 API 요청해보기
```
!cat ./call_chat_completion.sh
```

```shell
#!/usr/bin/env bash


curl https://api.openai.com/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -d '{
    "model": "gpt-3.5-turbo",
    "messages": [
      {
        "role": "system",
        "content": "너는 감정분류를 수행한다. user의 텍스트의 감정이 긍정이면 positive, 부정적이면 negative를 출력한다."
      },
      {
        "role": "user",
        "content": "이 영화는 정말 멋진것 같아."
      }
    ]
  }'
```
-H 옵션은 요청헤더를 추가할 때 사용하며, -d 뒤에는 json형식의 데이터가 오며, 요청 데이터 설정시 사용한다.

``` python
# 'call_chat_completion.sh'라는 파일의 실행 권한을 줌
!chmod +x ./call_chat_completion.sh
```
```shell
# 아래 코드로 실행
!./call_chat_completion.sh
```
## API 사용 2 - Python SDK로 API 요청해보기

```python
!pip install openai # 라이브러리 설치

from openai import OpenAI
client = OpenAI()
# 기본적인 사용법
completion = client.chat.completions.create(
  model="gpt-4",  # gpt-3.5-turbo, gpt-4, gpt-4-1106-preview, etc...
  messages=[
    {"role": "system", "content": "너는 도움이 되는 AI어시스턴트이다."},
    {"role": "user", "content": "너비우선탐색 알고리즘에 대해서 설명해줘"}
  ]
)
```
```python
print(completion.choices[0].message) # 답변출력
```
### 답변
ChatCompletionMessage(content='너비우선탐색(Breadth-First Search, BFS) 알고리즘은 그래프를 탐색하는 데 사용되는 알고리즘 중 하나입니다. 이 알고리즘은 시작 노드로부터 가까운 노드부터 차례대로 탐색하며, 모든 인접한 노드를 우선적으로 방문하는 특징을 가지고 있습니다.\n\n먼저 시작 노드를 큐(queue)에 넣고, 해당 노드를 방문했다고 표시합니다. 그 후 큐에서 하나의 노드를 꺼내어 해당 노드와 인접한 노드들을 모두 큐에 넣고, 방문한 노드로 표시합니다. 이 과정을 큐가 빌 때까지 반복하여 모든 노드를 방문합니다.\n\n너비우선탐색 알고리즘은 최단 경로를 찾는 문제나 두 노드 간의 최소 연결 경로를 찾는 문제에 유용하게 활용됩니다. 또한, 그래프의 구조를 확인하거나 특정 노드를 찾는 데도 사용할 수 있습니다.', role='assistant', function_call=None, tool_calls=None)


## 간단한 챗봇 만들기
```python
system_instruction = """
너는 영문 번역 AI비서야

영어로 질문하면, 영어로 답하고, 틀린 영문법을 고쳐줘
"""
```

```python
messages = [{"role": "system", "content": system_instruction}]

def ask(text):
    user_input = {"role": "user", "content": text}
    messages.append(user_input)

    response = client.chat.completions.create(
        model="gpt-3.5-turbo",
        messages=messages)

    bot_text = response.choices[0].message.content
    bot_resp = {"role": "assistant", "content": bot_text}
    messages.append(bot_resp)
    return bot_text
```
ask 함수안에 chat gpt에게 보낼 텍스트를 넣어 실행하면서 메시지를 주고 받는다. 대화에 대한 내용은 messages리스트 안에 저장된다.
```python
while True:
    user_input = input("user: ")
    bot_resp = ask(user_input)
    print(f"bot: {bot_resp}")
```