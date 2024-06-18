---
layout: default
title: Regions & Zones
parent: GCP
nav_order: 1
---

```
이 글은 udemy의 GCP Professional CLoud Architect: Google Cloud  Certification 강의를 바탕으로 작성되었습니다.
```

## aplication이 배포된 data center에 지진이 나서 문제가 발생하며 어떻게 될까?

이때 서비스가 멈추는 문제를 우리는 막기 위해, HA를 고려한 설계를 하게 된다. 이를 위해 **multi zone**을 사용하게 된다.  
또한, 한 region의 모든 zone에 문제가 생기는 한 region 전체가 문제가 발생해도 서비스가 이루어질 정도로 HA가 중요한 서비스의 경우는 multi region을 고려해볼 필요가 있다. (대신 비싸다.)

```
 용어 정리)  
 한 region 안에 여러 zone이 있다.  
 region이 london이면 london 안에도 여러 zone이 있어 분산 배포할 수 있다.
```

## 사용자의 다양한 지역에 있고 latency가 중요한 시스템에서 시스템을 어떻게 배포해야할까요?

이때는 위해 **multi region**을 사용한다. 다양한 지역의 사용자의 네트워크 latency를 최소화할 수 있다. 단지 multi region은 비용이 비싸므로 요구사항을 꼭 확인해야한다.
