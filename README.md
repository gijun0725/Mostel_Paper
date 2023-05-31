# Mostel_Paper
Mostel Code & Paper review


*용어 정리
---
| name   | Full_name   | Description   |
|-------|-------|-------|
| STE | Scean text editing | 백그라운드와 스타일을 유지하며 다른 텍스트로 바꾸는 작업 |
| MOSTEL | Modifiying Scene Text image at Stroke Level | 이미지에 있는 텍스트를 Stroke level로 변환 |
| SLM | Stroke level modification |  이미지의 선명도와 선의 굵기를 수정하여 이미지의 외형을 변화시키는 작업|


Abstract
---
  -stroke guidance maps를 통해서 수정하고자하는 영역을 지정
  
  -모든 픽셀을 고려해서 하는 작업이 아니다 텍스트가 있는 부분에 집중
  
  -합성이미지와 실제사진의 학습을 위해 반지도 학습을 진행
  

Introduction
---
  -목적 : 백그라운드 이미지와 기존의 텍스처와 텍스트 스타일을 유지하면서 텍스트를 수정
  
  -장점 : 어떠한 단어라도 일관성있게 수정가능하고 이미지내에 모든단어도 가능
  
  
  -STE 단점
  
- 백그라운드 텍스처를 보존하기 어렵습니다.
- 훈련 데이터가 합성 이미지이고 실제 이미지와 차이가 있어서 도메인 간격(Domain gap)이 발생하며, 실제 추론(inference) 시에 문제가 발생할 수 있습니다.
- 이미지 전체 픽셀을 고려해야 하는데, 이는 텍스트가 수정되는 동안에도 백그라운드가 바뀌면 안되기 때문에 어려움이 있습니다.
- 두 가지 작업을 하나로 합치다 보니 결과도 좋지 않고, 학습 데이터로 인해 편향(bias)된 결과가 나와서 실제 사진에 부적합한 결과를 얻을 수 있습니다.

  
  ---
  
