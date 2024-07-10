---
title: '[Streamlit] 간단한 에코봇 구현'
author: baduk
date: 2024-03-24 22:16:00 +0900
categories: [Framework, Streamlit]
tags: [Streamlit]
---
## 에코봇
```python
####
import streamlit as st

st.title("Echo Bot")

# Initialize chat history
if "messages" not in st.session_state:# stramlit이 새로고침 되더라도 화면 유지
    st.session_state.messages = []

# Display chat messages from history on app rerun
for message in st.session_state.messages: # 히스토리 메시지 디스플레이
    with st.chat_message(message["role"]): # {"role":"assistant", "content":"hello"}
        st.markdown(message["content"])

# React to user input
# prompt = st.chat_input("what is up?")
# if prompt:
if prompt := st.chat_input("What is up?"):
    # Display user message in chat message container
    st.chat_message("user").markdown(prompt) # 방금 막 보낸 메시지 디스플레이
    # Add user message to chat history
    st.session_state.messages.append({"role": "user", "content": prompt}) # 내 메시지 add

    response = f"Echo: {prompt}"
    # Display assistant response in chat message container
    with st.chat_message("assistant"):
        st.markdown(response) # 봇이 보낸 메시지 디스플레이
    # Add assistant response to chat history
    st.session_state.messages.append({"role": "assistant", "content": response}) # 봇 메시지 add
```