---
layout: default
title: K8S Deployment Strategy
parent: K8S
nav_order: 1
---
# K8S 배포 전략

## recreate

Downtime 발생

## 롤링 업데이트

점진적으로 업데이트 하는 것으로 느리다. 

두가지 버전이 일시적으로 공존하여 공존하면 안되는 버전일 경우 사용이 불가능하다.

### 사용법

```bash
kubectl rollout

kubectl rolloout undo

kubectl rollout history
```

## 블루/그린

블루(기존 버전)과 그린(새 버전)을 둘 다 띄워 놓고 블루에서 그린으로 routing 을 바꾼 후 이상이 없으면 블루를 지우는 배포 방식

### 장점

1). 기존 버전의 Pod 를 즉시 지우지 않아 라우팅을 바꾸기 전에 기존 버전 Pod에서 처리 중이던 응답이 종료되지 않아 일시적 오류를 발생시키지 않음

2) 오류 발견시 즉시 기존 버전으로 바꿀 수 있다.

### 단점

1) 2개의 버전이 동시에 배포되어 있어 배포 작업 동안에는 자원이 2배가 쓰인다.

### k8s에서 사용법

k8s는 service가 selector로 label로 routing을 결정한다

```bash
spec:
  selector:
    app: myApp
    version: 2
```

이런식으로 selector를 구성하여 service의 version만 변경하여 라우팅을 바꾸게 한다.

### 주의 사항

트래픽이 많이 발생할때 새로운 버전으로 라우팅을 바꾸게 되면 새로운 app이 초기에 db connection 준비와 초기 앱이 뜰때  memory 사용량이 많은 경우에는 갑자기 트레픽이 쏠 리면 에러가 발생할 수 있다.

## 카나리 배포

일부 트래픽만 새로운 버전으로 라우팅되게 하고 이상이 없으면 새로운 버전으로 바꾼다.

### k8s에서 사용법

방법1 .

새 버전의 deployment를 띄우고 label을 기존 label가 같게하여 service로 부터 route 되도록한다.수량은 replica로 조절하면된다.

방법2.

version1, version 2를 각각 service를 만들고 nginx ingress와 istio ingress 를 통해 routing 비율을 조절하며 트레픽을 보낼 수 있다. (A/B 테스트와 사용 방법 동일)

## 가장 안정적인 배포 방법: blue/green 배포+ 카나리 배포

이전 버전과 새버전을 띄워 놓은 상태에서 이전 버전의 일부만 새 버전으로 바뀌는 카나리 배포를 사용하고 이상없으면 green으로 routing되도록한다.
