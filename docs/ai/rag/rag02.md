---
title: ReAct
layout: default
parent: RAG
grand_parent: AI
nav_order: 2
---

# ReAct

## 논문

 ReAct: Synergizing Reasoning and Acting in Lauguage Models

https://arxiv.org/pdf/2210.03629

## 동작

1. Reasoning
2. Acting
3. 1~ 2 반복

## 장점

- api 를 통해 정보를 가져오고 그 정보를 바탕으로 답변생성을 하여 hallucination을 해결
- chain-of-thought의 오류 전파 방식을 줄임(error propagation)
    - 잘못된 검색 결과를 토대로 추론을 계속할 경우, 잘못된 응답을 낼 가능성이 높음. ReAct의 경우 검색 결과를 바탕으로 추론을 하고 추가적인 Act 작업을 계속하면서 적절한 답을 찾아냄

## 단점

- 여러번의 llm과 검색 호출로 응답시간이 오래 걸림

## CoT vs Acting vs ReAct

![](../../../../assets/ai/rag/images/rag02-example.png)
