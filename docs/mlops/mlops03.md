---
layout: default
title: 데이터 드리프트
parent: MLOPs
nav_order: 3

---
# 데이터 드리프트

모델의 입력의 데이터 분포 또는 모델 출력 데이터의 분포가 달라지는 지는 걸 데이터 드리프트라고 한다. 
세상이 변화하면서 모델의 입력, 출력 데이터가 달라질 수 있다. 

예를들면 쳇봇에서 새로운 용어가 생겨 그에 관련한 질문이 들어온다는지 변화하는 세계로 인해 데이터 드리프트가 발생한다 

따라서 모델을 배포 후에 서비스에서 데이터 드리프트가 발생하는지 모니터링이 필요하다.
그 모니터링을 통해 모델 재학습 필요 여부를 확인할수있다
