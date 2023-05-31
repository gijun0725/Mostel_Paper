# Mostel_Paper
Mostel Code & Paper review


### 용어 정리

| name   | Full_name   | Description   |
|-------|-------|-------|
| STE | Scean text editing | 백그라운드와 스타일을 유지하며 다른 텍스트로 바꾸는 작업 |
| MOSTEL | Modifiying Scene Text image at Stroke Level | 이미지에 있는 텍스트를 Stroke level로 변환 |
| SLM | Stroke level modification |  이미지의 선명도와 선의 굵기를 수정하여 이미지의 외형을 변화시키는 작업|
| BRM | Background Reconstruction Module | 백그라운드만 뽑아내기 위한 모듈|
| TMM | Text Modification Module | 텍스트만 뽑아내기 위한 모듈|
| SLM | SLM | 백그라운드 필터링 하는 모듈 |
| TPS | Thin Plate Splines | Is의 기하학적 속성에 따라 It의 방향을 조정 |



# Abstract

- stroke guidance maps를 통해서 수정하고자하는 영역을 지정
- 모든 픽셀을 고려해서 하는 작업이 아니다 텍스트가 있는 부분에 집중
- 합성이미지와 실제사진의 학습을 위해 반지도 학습을 진행

# Introduction

### 목적
백그라운드 이미지와 기존의 텍스처와 텍스트 스타일을 유지하면서 텍스트를 수정하는 것을 목적으로 한다.

### STE 단점
- 백그라운드 텍스처를 보존하기 어렵다.
- 훈련 데이터가 합성 이미지이고 실제 이미지와 차이가 있어서 도메인 간격(Domain gap)이 발생하며, 실제 추론(inference) 시에 문제가 발생할 수 있다.
- 이미지 전체 픽셀을 고려해야 하는데, 이는 텍스트가 수정되는 동안에도 백그라운드가 바뀌면 안되기 때문에 어려움이 있다.
- 두 가지 작업을 하나로 합치다 보니 결과도 좋지 않고, 학습 데이터로 인해 편향(bias)된 결과가 나와서 실제 사진에 부적합한 결과를 얻을 수 있다.

### MOSTEL 장점
- 텍스트가 있는 영역만을 고려하여 수정하기 때문에 학습하기 더 쉽다.
- 기존에는 한 번에 수정하다 보니 배경도 같이 계속 수정되는데, MOSTEL은 배경과 텍스트를 따로 작업하기 때문에 배경의 변동이 적다.
- 반지도 학습으로 Domain gap을 줄인다 즉 정답값이 없는 데이터 사용(real-image)
- 이미지에서 원래 있던 텍스트를 지우고 다시 그텍스트를 복구하는 작업이 진행된다. (이해하기 쉽게 수학문제를 지우고 다시한번 문제를 써보는 작업이라고 이해하면 된다)
-생성된 텍스트 이미지의 가독성을 보장하기 위해 scene text 인식기를 사용
-합성이미지 데이터셋과 실제이미지 데이터셋 두가지가 있다.(편향 방지)
-위의 데이터는 STE 평가 데이터셋으로, STE 방법들의 공정한 비교를 크게 용이하게 한다.

# Realated Work

### Style Transfer

- [(Zhu et al. 2017] 사이클 일관성 손실을추가하여 도메인사이에서 일관성을 보장
- [Yang et al. 2017] 거리기반의 특성으로 최초의 스타일 트랜스퍼 적용
- [(Yang et al. 2019a)] 스타일화 및 비스타일화 서브네트워크를 사용하여 양쪽의 작업을 모두 수행

### Scene Text Editing
- SRNet first proposes the word-level editing method[Only simple task]-SRNet을 사용하지 않은 결정적인 이유
- STRIVE (Subramanian et al. 2021)의 영상을 참고하면 된다.

### Methodology

-MOSTEL은 가독성이 좋지않은 글자를 타겟으로 하였기에 기존의 SRNet의 업그레이드 버전이라 할 수 있다.
-erase-and-write paradigm [unpaired 방법이라고 이해]
-design a novel stroke-level modification[좀더 심화된 SRNet 방법]
-Is 의 인풋결과 Guides[Only text]와 Os[Only Background]가 출력
  
