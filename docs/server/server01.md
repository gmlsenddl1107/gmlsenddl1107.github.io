---
layout: default
title: LCEL (Langchain) - 시작하기
parent: 서버
nav_order: 1
---

# LCEL (Langchain) - 시작하기

AI 모델을 활용한 애플리케이션을 만드는 framework이며 주로 언어모델을 활용하기 쉽게 구성되어있다.

앞으로 알아볼 내용은 LCEL(LangChain Expression Lauguge)를 사용하여 chain을 구성하는 방법이다.  
LCEL은 LangChain component을 chaining하기 쉽게 만들어진 언어다.  
특히, streaming, async, parallel execution 지원된다.

이 문서를 아래 버전을 바탕으로 작성되었습니다.

```
langchain-community==0.0.34
langchain-core==0.1.45
langchain-openai==0.1.3
langsmith==0.1.49
```

# Langchain Component

chain을 구성할 수 있는 component는 다양하게 있으며 [https://python.langchain.com/docs/integrations/chat/](https://python.langchain.com/docs/integrations/chat/)에서 확인이 가능하다. 

-   PromptTemplate  
    input에 따라 promptTemplate를 채워 prompt 를 만들수 있는 class (사실,,, f string 과 비슷하다.)
-   Chat models
-   Document Loadder
-   Tool
-   Memory
-   등..

# chain을 연결하는 방법

```
chain = prompt | model | output_parser 
output = chain.invoke({"user_utterance":"hello"})
```

`|` 기호를 통해 서로 다른 구성 요소를 연결하고 각 요소의 output을 다음 요소의 input으로 사용

# 의문점

처음 LCEL을 접했을 때 가장 이해가 어려웠던 부분이 `|` 로 chain 이 연결되는 부분이였다.

**의문1. 어떻게 구현되어 있는건지**  
runnable 프로토콜을 구현하여 `|` 로 chain을 구성하였다고 하는데 도대체 어떻게 동작하고 어떻게 구현을 한지 감이 잡히지 않았다.

[2024.04.21 - \[ML SYSTEM 설계\] - LCEL (Langchain) - 의문1. 어떻게 구현되어 있는건지](https://cheer-up-programmer.tistory.com/270)

**의문2. 에러 발생시 어떻게 처리되는지.**  
| 로 연결된 chain을 중간단계에서 에러가 발생하면 어떻게 처리가 되는지 궁금했다.

**의문3. production level에서 사용이 가능한지.**  
간단한 프로토타입을 만들기는 좋아보이나 production level에서 사용이 가능한지 궁금했다. 특히, 기획자의 요구사항에 따라 변경이 쉬운 flexible한 형태인지가 궁금했다.

**의문4. chain을 구성해서 사용할 때와 아닌 경우의 속도차이가 없는지.**  
langchain 내부에서 실행되는 불필요한 기능들 때문에 같은 기능을 필요한 부분만 직접 구현해서 사용할 때 속도 차이는 없는지 궁금했다.

# 코드 까보기

궁금해서 하나씩 코드를 까보기 시작하였다. 의문별로 글을 쓸예정이니 다음 글을 확인하세요.

[2024.04.21 - \[ML SYSTEM 설계\] - LCEL (Langchain) - 의문1. 어떻게 구현되어 있는건지](https://cheer-up-programmer.tistory.com/270)