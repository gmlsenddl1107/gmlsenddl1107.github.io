---
title: Overfitting
layout: default
parent: AI base
grand_parent: AI
nav_order: 1
---
# Overfitting

## Overfitting 확인 방법

trainset loss는 줄어들거나 제자리인 상태에서 validation loss가 느리게 줄어들거나 제자리 또는 증가하면 overfitting되고 있을 가능성이 있다.

## 방지 방법

1. 좋은 학습 데이터 만들기
2. 정규화
    1. L1 
        1. weight의 절대값에 비래해 손실함수에 패널티를 추가 일부 weight가 0이되는 효과
    2. L2
        1. weight의 제곱에 비례해 손실함수에 패널티
    3. dropout
    4. max-norm
    5. 배치 정규화
3. 모델 크기 축소
    1. 모델이 작을 수록 기억할 수 있는 정보의 양이 줄어들기 때문에
4. early stopping
