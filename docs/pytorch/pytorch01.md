---
layout: default
title: TorchScript
parent: Pytorch
nav_order: 1
---
# TorchScript

Torchserve를 사용하려면 모델 파일(bin), handler  등을 torch-model-archiver로 MAR 파일를 만들어야한다. 이때  모델을 Eager 모드로 사용할 수 도 있고  script 모드로 사용할 수 있다.

pytorch를 서빙에서 eager mode -> script mode로 바꾸기 위해서 아래 2가지 방법이 있다.

1. scripting 방식
    
    torch.jit.script()
    
2. tracing 방식
    
     torch.jit.trace() 어떤 입력 값을 사용하여 모델 구조를 파악하고 모델 안에서의 흐름을 기록하는 방식 조건문을 많이 사용하지 않는 모델에 적합
    
    문제점: control flow나 python construct가 보존되지 않아 올바르게 보존되는지 확인 필요
    

## Torchsript 장점

- Jit compile(just in time compilation)은 pytorch model을 intermediate representation으로 변경하여 효과적으로 실행. python 코드가 IR로 변환되기 떄문에 python depency가 사라짐
- 정적 그래프로 실행되어 실행 속도가 빨라짐


