---
title: Langchain 기초
layout: default
parent: RAG
grand_parent: AI
nav_order: 1
---

# Langchain 기초

```
 잘못된 내용이 있으면 댓글로 알려주시면 감사합니다.
 언제나 기술적인 피드백은 환영합니다.
```

## Langchain 구성 요소

- Model I\O
    - Prompt
    - Languages models
- Data connection
    - Document loaders
    - Document transformers
    - Text embedding model
    - Vector store
    - Retriever
- Chains
- Agents
    - Agent types
    - Tools
    - Toolkits
- Memory
- Callback

## Retrival

### Document loaders

사용 목적 : 데이터를 가져온다

### Document transformers

사용 목적: 데이터를 LLM모델에 넣을때 답변하기 좋은 형태, Vector 검색이 잘되는 형태로 변환한다

chunk: 문서를 적절한 크기로 분할(LLM 모델의 input token 길이는 한정되어 있어 고려 필요)

### Text embedding model

저장할 chunk를 벡터화한다

### Vector store(Vector DB)

vector를 저장할 곳

- 크로마
- qdrant
    - es의 filter 기능이 있어 custome 용이
    - 속도가 빠르다.
    - Cosine stimilarity, 유클리드 distance 등 지원
- Faiss
- redis

### Retriever

사용자의 질문과 비슷한 문서를 가져오는 인터페이스

여러 알고리즘을 이용하여 가장 가까운 문서를 찾아주는건 사실 vector DB가 지원해준다
