---
layout: default
title: Flash attention
parent: Serving
nav_order: 2
---
# Flash attention

```
 잘못된 내용이 있으면 댓글로 알려주시면 감사합니다.  
 언제나 기술적인 피드백은 환영합니다.
```

gpu bandwidth의 한계를 극복하기 위해 attention 계산 순서를 재정렬하고 tiling과 재연산을 활용하여 속도를 높인 알고리즘

즉,  tiling으로 데이터를 작은 단위로 쪼개서 SRAM을 최대한 활용하면서 attention 계산을 QK 계산부터 softmax까지 다 한 결과물을 HBM에 저장하게 하여 속도를 높임. (기존에는 중간 연산 결과를 중간 중간 HBM에 저장하고  읽어 오기를 반복했음).

### tiliing란?

tiling은 큰 매트릭을 작은 sub 매트릭 즉 tile로 쪼갠하는 의미.
작은 chunk 단위로 data를 쪼개어 계산하여 SRAM을 활용하여 최대한 HBM에 연산 과정을 적을 일을 줄인다.

- HBM = GPU 매모리
- SRAM = fast cache

메모리 사용량은 sequence len의 제곱에서 squece len으로 낮춤

## 특징

- train에서도 사용됨
- mutli head attention을 fused kernel로 만듬→메모리에 읽고 쓰는 것을 fused kernle을 통해 최소화함
- gpu bandwidth의 한계를 극복

## Multi Head Attention 계산 과정

1. Q, K를 HBM에서 읽고 S=QK^T를 계산 후 , S를 HBM에 적기
2. S를 HBM에서 읽어서 P=softmax(S)를 계산 후에 P를 HBM에 적기
3. P,V를 HBM에서 읽고 O=PV를 계산 후에 O를 HDM에 적기

HBM은 GPU SM 칩안에 있는것이 아니기 때문에 느리다.

HBM에서 읽어오는 복잡도는 seq len의 제곱에 비례한다.

## Flash attention  계산 과정

계산 결과가  달라진 것이 아닌 HBM의 읽고 쓰기를 최소화한 알고리즘이다.

1. Q,K를 HBM에 읽어와서
2. Q,K를 곱하고 그 값을 계속 들고 있으면서 softmax를 계산한 결과를 HBM에 저장한다

QK의 곱한 값을 들고 있으면서 softmax를 계산하기 위해 tiling이라는 기법을 사용했다고 한다.
그 과정에서 HBM에 읽고 쓰는것을 최소화하여 빨라짐

## Flash Attention2

matmul 연산 외의 연산을 최소해함(GPU 처리량 극대화)
Multi query attention, Group query attenction에 최적화
Flast attention2는 flash attention보다 2배 빠르고 그냥 attention보다 9배 빠르다고 한다.

## Flash Attention3

hopper GPU에서만 사용 가능
