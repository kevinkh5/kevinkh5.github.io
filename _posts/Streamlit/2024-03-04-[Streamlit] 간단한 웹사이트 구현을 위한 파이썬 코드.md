---
title: '[Streamlit] 간단한 웹사이트 구현을 위한 파이썬 코드'
author: baduk
date: 2024-03-04 20:13:00 +0900
categories: [Skill, Streamlit]
tags: [Streamlit]
---

```python
import streamlit as st

st.title("Title")
st.subheader("Subheader")
st.write("Write")

"""
# 안녕하세요.
## 반갑습니다.
- 잘부탁드려요
"""

text = st.text_input("텍스트 입력")
st.write(text)

gender = st.selectbox('성별',('남자','여자'))
st.write(f"성별:{gender}")

selected = st.checkbox("동의하시겠습니까?")

if selected:
    st.success('동의합니다.')
else:
    st.warning('동의하지않습니다.')

options = st.multiselect('취미',['음악 감상','독서','게임'])
st.write(', '.join(options))

with st.sidebar:
    add_radio = st.radio("언어 선택", ("한국어","영어"))

col1, col2, col3 = st.columns(3)

tab1,tab2,tab3 = st.tabs(['cat1','cat2','cat3'])

with tab1:
    st.header("A cat1")
    st.image("https://i.imgur.com/iQsFioD.gif")
with tab2:
    st.header("A cat2")
    st.image("https://64.media.tumblr.com/tumblr_mej2xq5da51rxis0k.gif")
with tab3:
    st.header("A cat3")
    st.image("https://i.imgur.com/nQ50U.gif")
```