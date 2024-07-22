---
layout: default
title: Torch.compile
parent: Pytorch
nav_order: 2
---

# Torch.compile

Torch.compile은 pytorch 2.0 부터 등장하여 모델을 optimized kernel로 바꾸는 등 최적화 작업
Torch.jit.script도 모델을 최적화 하지만 배포용으로 최적화하는 거라 용도에 맞게 쓰면 됨

### 단점

compile 자체가 오래 걸리는 작업인데 dynamic input shape의 경우 shape 바뀔 때마다 compile을 다시 해주어야 함.
