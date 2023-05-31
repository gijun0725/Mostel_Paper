# Mostel_Paper🖌️
Mostel Code & Paper review


### 용어 정리🕮

| name   | Full_name   | Description   |
|-------|-------|-------|
| STE | Scean text editing | 백그라운드와 스타일을 유지하며 다른 텍스트로 바꾸는 작업 |
| MOSTEL | Modifiying Scene Text image at Stroke Level | 이미지에 있는 텍스트를 Stroke level로 변환 |
| SLM | Stroke level modification |  이미지의 선명도와 선의 굵기를 수정하여 이미지의 외형을 변화시키는 작업|
| BRM | Background Reconstruction Module | 백그라운드만 뽑아내기 위한 모듈|
| TMM | Text Modification Module | 텍스트만 뽑아내기 위한 모듈|
| SLM | SLM | 백그라운드 필터링 하는 모듈 |
| TPS | Thin Plate Splines | Is의 기하학적 속성에 따라 It의 방향을 조정 |
| SHL | Semi-supervised Hybrid Learning | 반지도 학습을 의미한다 |

### Model Architecture
![image](https://github.com/gijun0725/Mostel_Paper/assets/119472512/dddb18d2-6eb9-4123-bbdc-14cf19749159)




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

- MOSTEL은 가독성이 좋지않은 글자를 타겟으로 하였기에 기존의 SRNet의 업그레이드 버전이라 할 수 있다.
- erase-and-write paradigm [unpaired 방법이라고 이해]
- design a novel stroke-level modification[좀더 심화된 SRNet 방법]
- Is 의 인풋결과 Guides[Only text]와 Os[Only Background]가 출력
- 텍스트의 특성을 강하게 가지고 있는 Is와 Is의 기하학적 특성을 적용한 It는서로 합쳐진다.

### BRM[Background Reconstruction Module]

- 인코더 디코더의 구조로 되어있고 좀더 확실한 이미지를 얻기위한 up-sampling이 적용된다.
- 텍스트를 바꾸면서 백그라운드를 유지하고 싶지만 두개가 한번에 작업되다 보니 서로에게 영향이 있을수 밖에 없기때문에 SRNet도 어느정도 번짐이나 Blur같은 현상을 피할수 없다.
- 이를 해결하기 위한 개념은 O^s와 Guide_s를 통하여 SLM(백그라운드 필터링 하는 모듈)을 통과시켜 텍스트가 있는 부분을 알려줘서 불필요한 부분과 필요한 부분에 대하여 백그라운드를 생성한다.
- Os는 GAN loss와 L2 loss를 사용한다

### TMM[Text Modification Module]

- Pre-Transformation and Modification Module 은 전처리 영역이라고 생각하면된다.
- FC에서 글자의 위치를 잡기위해 anchor점들을 얻어내고 TPS에서 이점을 기준으로 I_t의 텍스트의 위치를 재조정 한다
- 위에서 언급했듯이 백그라운드를 제거한 텍스트만 있는 이미지를 만들어 낸다 추가적으로 Augmentaion이 진행되는데 이는 이전에서 TPS에서의 위치값으로 I_t가 미리 변경되기 때문에 Augmentation을 진행한다고 해서 위치좌표값이 변하지 않는다.

![image](https://github.com/gijun0725/Mostel_Paper/assets/119472512/038ce4a7-ff77-43f1-b95c-dfa7e4a38264)


### MM[Modification Module]

- Up-sampling을 통하여 O^t와 Guide_t를 생성해 낸다.
- The gradient of connections is blocked 이문장은 skip-connection과 같은 역할로 입력값과 똑같은 텍스트가 나오지 않게 하기위해 사용하는 방식으로 기울기 소실 문제를 해결한다.
- SLM은 이러한 정보를 가지고 텍스트와 백그라운드를 더 잘구분할 수 있는것이다. 

### SHL[Semi-supervised Hybrid Learning]

- 실제 이미지에서는 라벨값이 없기때문에 기존 이미지에서 텍스트를 지운후에 기존의 즉 I_s가 지워진 상태에서 다시 I_s를 재구성하기 때문에 합성이미지와 구조는 같지만 다른 의미를 가지며 이때 사용하는 Loss는  Lrec (재구성 손실)과 Lvgg (스타일 손실) 두가지로 구성된다.
- 위에서 언급한 방법은 다른이미지도 똑같은 결과를 초래 할 수 있는 항등매핑의 가능성이 있어서 단점이라고 볼 수 있다.
- 학습에는 14개의 합성이미지 2개의 실제이미지를 이용하였다
![image](https://github.com/gijun0725/Mostel_Paper/assets/119472512/d300ed15-a244-4cbf-aa8a-28c6cf4b0ae2)


# Experiment
### Dataset
Synthetic Data (합성 데이터)

- Supervised Training용으로 15만 개의 라벨이 달린 이미지를 생성
- Tamper-Syn2k 평가를 위해 2,000개의 이미지를 생성
- 생성된 이미지는 다른 텍스트를 가지지만 동일한 스타일(폰트, 크기, 색상, 공간 변환, 배경 이미지)을 가진다.
- 300개의 폰트와 12,000개의 배경 이미지를 사용하며, 무작위로 회전, 곡선 및 원근 변환을 적용


Real Data (실제 데이터):
- MOSTEL을 실제 세계의 씬 텍스트 이미지에 대해 학습하기 위해 MLT-2017 데이터셋을 사용
- 이 데이터셋에는 34,625개의 이미지가 포함.
- 텍스트 영역을 지정하고 인식기 손실을 계산하기 위해 텍스트 주석(annotation)만 필요
- 평가를 위해 Tamper-Scene 데이터셋을 사용하며, 이는 ICDAR 2013, SVT, SVTP, IIIT, MLT-2019 및 COCO-Text 등의 여러 씬 텍스트 데이터셋을 조합
- 심하게 왜곡되거나 인식하기 어려운 이미지는 필터링하여 총 7,725개의 이미지로 구성


### Evaluation metrics
- PSNR (Peak Signal-to-Noise Ratio): PSNR은 이미지 또는 비디오의 품질을 측정하는 데 사용되는 지표이다. PSNR이 높을수록 원본과의 차이가 적어지고, 더 나은 품질을 가진다고 할 수 있다.
- MSE (Mean Squared Error): MSE는 이미지 또는 비디오의 픽셀 간 차이를 측정하는 지표이다. MSE가 낮을수록 원본과의 차이가 적어지고, 더 나은 품질을 가진다고 할 수 있다.
- SSIM (Structural Similarity Index): SSIM은 이미지 품질의 구조적 유사성을 측정하는 지표 로서 두 이미지 간의 구조적 유사성을 비교하여 더 높은 SSIM 값은 더 나은 품질을 나타낸다.
- FID (Fréchet Inception Distance): FID는 InceptionV3 모델로 추출된 특징들 간의 거리를 측정하는 지표이다. 낮은 FID 값은 더 나은 품질을 나타내며, 이미지들의 특징이 더 유사하다는 것을 의미한다.

### 실제 데이터 비교 [각 기능을 추가하거나 제거했을 경우]
![image](https://github.com/gijun0725/Mostel_Paper/assets/119472512/c176e736-987c-434f-8705-da9b81efd382)
- w/o SLM : SLM 사용하지 않음
- w/o BF : Background Filtering사용하지 않음[이런 경우 똑같은 이미지 생성가능성 있다]
- w/o SA : Style augmentaion 사용하지않음[이런 경우 똑같은 이미지 생성가능성 있다]
- w/gradient : 기울기를 추가하면 똑같은 이미지 나올 수 있음[이런 경우 똑같은 이미지 생성가능성 있다]
- w/o : recognizer를 제외했을경우[인식률이 떨어진다]
- MOSTEL: 위에서 모든걸 적용하였을경우[가장 좋은 케이스]

# Conclusion

- 이 연구에서는 MOSTEL이라는 씬 텍스트 편집을 위한 엔드 투 엔드 학습 가능한 프레임워크를 사용.
- 한정된 성능을 편집 가이드와 합성 훈련 데이터와 실제 씬 텍스트 이미지 간의 도메인 간격으로 귀속
- 따라서 Stroke-Level Modification을 도입함으로써, 분산을 제거하고 모델이 텍스트 영역의 편집 규칙에 집중하도록 명시적으로 가이드하는 것을 제안한다. 
- 또한, 반지도 하이브리드 학습을 도입하여 신경망이 짝지어진 합성 이미지와 라벨이 없는 실제 세계 이미지로 훈련될 수 있도록 한다.
- 배경 필터링, 스타일 보강 및 무경사한 연결과 같은 여러 가지 방법을 도입하여 모델을 이러한 체계에 적응시킨다.
- MOSTEL 외에도 Tamper-Syn2k 및 Tamper-Scene이라는 두 가지 평가 데이터셋을 공개하여 객관적 평가를 진행했다.
