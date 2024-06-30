---
layout: default
title: 딥러닝 시스템 서빙 용어 정리
parent: Serving
nav_order: 1
---

```
 잘못된 내용이 있으면 댓글로 알려주시면 감사합니다.  
 언제나 기술적인 피드백은 환영합니다.
```

여러 책을 공부하면서 헷갈리는 언어가 있었는데 아래 단어이다.

1\. model serving vs model prediction vs model inference

2\. online inference vs online serving vs batch prediction vs batch serving

## model serving vs model prediction vs model inference

### model serving

:모델을 배포하여 실시간 요청 또는 일괄 처리를 하도록하는 과정이다. model inference을 포함하는 개념이다.

### model prediction

: 모델이 output 값을 만든는 것. auto regression model인 llm 관점에서는 다음 토큰을 생성하는 것이다.

### model inference

model prediction 보다 좀더 포괄적인 관점으로 llm 관점에서는 생성해야할 전체 토큰을 다 생성하여 응답을 만든 걸 의미한다.또한, 모델 input의 preprocessing과 post processing도 포함해서 이야기 되기도 한다.

**결론적인 model serving > model inference > model predition 을 포함하는 개념인거 같다.**

## online inference vs online serving vs batch prediction vs batch serving

### online prediction (온라인 예측)

: 구글 번역과 같이 사용자의 요청에 대해 실시간으로 모델을 inference한다.

~(사실.. online 서빙에서도 batch처리를 하는 데 왜 ㅠ 서빙 방법을 online prediction과 batch ~pre~diction으로 왜 나누는지 ㅜㅜ)~~ 

### online serving

: online prediction한 결과를 바로 응답할 수 있고, 또는 batch prediction 결과를 request 따라 실시간으로 저장소 읽어와 결과를 response 할 수 있다. 모델의 결과를 실시간으로 전달할건지 아닌지로 구분한다. 가끔 online serving 을 online prediction 의미로 사용하는 글도 있다....ㅠㅠ

~(online serving과 model serving도 ...헷갈리게 비슷한데 다른 용어다 ㅜㅜ)~

### batch prediction (배치 예측)

: 추천 시스템처럼 주기적으로 또는 트리거 될때 모델을 inference한다.

### batch serving

batch prediction 결과를 대규모로 처리하고 저장소에 저장하거나 전달하는 것을 의미한다.

주의)online prediction+ online serving, batch prediction+ batch serving만 있는 것이 아니고 batch prediction+ online serving 과 같이 모델 inference는 주기적으로나 트리거나 될때하지만 저장소에 저장해 두었다가 serving은 request 왔을때 실시간으로 하는 online serving 과 같은 형태도 있다.
