---
title: '[OpenAI-API] Assistant API 사용법 - Thread'
author: baduk
date: 2024-03-03 08:54:00 +0900
categories: [Project, Toy Project]
tags: [OpenAi]
---

## OpenAI 라이브러리 설치
```python
!pip install --upgrade openai
!pip show openai | grep Version
```

## Assistant API 사용
```python
import os
# client.api_key = os.getenv("OPENAI_API_KEY")
```

### Assistant 생성
```python
from openai import OpenAI

client = OpenAI()

assistant = client.beta.assistants.create(
    name="Math Tutor",
    instructions="너는 개인 코딩 과외 선생님이야. 질문에 한 문장 이하로 간단하게 답해줘",
    model="gpt-4-1106-preview",
)
show_json(assistant)
```

### Threads 생성
```python
import json

def show_json(obj):
    display(json.loads(obj.model_dump_json()))

# Threads는 메시지를 관리하는 단위
thread = client.beta.threads.create()
show_json(thread)
```

```python
# 주의해야할 것은 위 코드 실행할 때마다 전체 대화 텍스트에 대한 토큰이 발생함.
message = client.beta.threads.messages.create(
    thread_id=thread.id,
    role="user",
    content="다이나믹 프로그래밍에 대해서 설명해줘"
)
show_json(message)
```

```python
run = client.beta.threads.runs.create(
    thread_id=thread.id,
    assistant_id=assistant.id,
)
show_json(run)
```

```python
import time

def wait_on_run(run, thread):
    while run.status == "queued" or run.status == "in_progress":
        run = client.beta.threads.runs.retrieve(
            thread_id=thread.id,
            run_id=run.id,
        )
        time.sleep(0.5)
    return run

run = wait_on_run(run, thread)
show_json(run)
```
run.status가 queued 또는 in_progress이면 잠시 대기


```python
#Run완료 후, Thread 안의 메시지들을 나열할 수 있다.
#가장최근결과가 위로 옴
messages = client.beta.threads.messages.list(thread_id=thread.id)
show_json(messages)
```

```python
# 추가적으로 메시지 보내기
# Create a message to append to our thread
message = client.beta.threads.messages.create(
    thread_id=thread.id, role="user", content="설명해 주시겠어요?"
)

# Execute our run
run = client.beta.threads.runs.create(
    thread_id=thread.id,
    assistant_id=assistant.id,
)

# Wait for completion
wait_on_run(run, thread)

# Retrieve all the messages added after our last user message
messages = client.beta.threads.messages.list(
    thread_id=thread.id, order="asc", after=message.id
)
show_json(messages)
```

## Threads 별로 메시지 보내고, Response 받기
```python
from openai import OpenAI

MATH_ASSISTANT_ID = assistant.id  # or a hard-coded ID like "asst-..."

client = OpenAI()

def submit_message(assistant_id, thread, user_message):
    client.beta.threads.messages.create(
        thread_id=thread.id, role="user", content=user_message
    )
    return client.beta.threads.runs.create(
        thread_id=thread.id,
        assistant_id=assistant_id,
    )


def get_response(thread):
    return client.beta.threads.messages.list(thread_id=thread.id, order="asc")

def create_thread_and_run(user_input):
    thread = client.beta.threads.create()
    run = submit_message(MATH_ASSISTANT_ID, thread, user_input)
    return thread, run


# Emulating concurrent user requests
thread1, run1 = create_thread_and_run(
    "방정식 '3x + 11 = 14'를 풀어줘"
)
thread2, run2 = create_thread_and_run("선형대수에 대해 설명해줘")
thread3, run3 = create_thread_and_run("나는 수학을 싫어해. 어떻게 하면 좋을까?")

# Now all Runs are executing...

import time

# Pretty printing helper
def pretty_print(messages):
    print("# Messages")
    for m in messages:
        print(f"{m.role}: {m.content[0].text.value}")
    print()


# Waiting in a loop
def wait_on_run(run, thread):
    while run.status == "queued" or run.status == "in_progress":
        run = client.beta.threads.runs.retrieve(
            thread_id=thread.id,
            run_id=run.id,
        )
        time.sleep(0.5)
    return run


# Wait for Run 1
run1 = wait_on_run(run1, thread1)
pretty_print(get_response(thread1))

# Wait for Run 2
run2 = wait_on_run(run2, thread2)
pretty_print(get_response(thread2))

# Wait for Run 3
run3 = wait_on_run(run3, thread3)
pretty_print(get_response(thread3))

# Thank our assistant on Thread 3 :)
run4 = submit_message(MATH_ASSISTANT_ID, thread3, "고마워")
run4 = wait_on_run(run4, thread3)
pretty_print(get_response(thread3))

## 출력결과
# # Messages
# user: 방정식 '3x + 11 = 14'를 풀어줘
# assistant: x = 1

# # Messages
# user: 선형대수에 대해 설명해줘
# assistant: 선형대수는 벡터공간과 선형변환을 다루는 수학의 한 분야입니다.

# # Messages
# user: 나는 수학을 싫어해. 어떻게 하면 좋을까?
# assistant: 수학을 더 흥미롭게 만들기 위해 실생활과 관련된 문제를 풀거나 게임 같은 수학 활동을 시도해 보세요.

# # Messages
# user: 나는 수학을 싫어해. 어떻게 하면 좋을까?
# assistant: 수학을 더 흥미롭게 만들기 위해 실생활과 관련된 문제를 풀거나 게임 같은 수학 활동을 시도해 보세요.
# user: 고마워
# assistant: 천만에요, 도움이 필요하면 언제든지 말씀해 주세요!
```