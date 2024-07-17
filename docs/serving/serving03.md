---
layout: default
title: vLLM
parent: Serving
nav_order: 3
---

# vLLM

### 특징

- page attenction
    - k,v를 cache하면서 메모리 사용량이 증가하는 단점을 보완하기 위해 나옴
    - k,v cache 뿐만 아니라 framentation으로 인해 60%~80%의 메모리를 더 사용하고 있음
    이를 해결하고자 나온 알고리즘
    → contigouous key, value를 non contiguous한 메모리 공간에 저장한다. 각  block 마다 정해진 key나 value token들을 가지고 있으며 paging을 통해 계산 시에 각 block을 효율적으로 가져다가 사용한다.
    → 메모리 제한으로 한번에 batch 처리 할수 있는 갯수의 제한이 있어 tps가 낮았던 상황에서는 tps가 많이 개선
- memory sharing을 통한 최적화
- in flight batching 지원
