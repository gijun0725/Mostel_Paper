# Mostel_Paper
Mostel transfer review and code description

| name   | Full_name   | Description   |
|-------|-------|-------|
| STE | Scean text editing | 백그라운드와 스타일을 유지하며 다른 텍스트로 바꾸는 작업 |
| MOSTEL | Modifiying Scene Text image at Stroke Level | 이미지에 있는 텍스트를 Stroke level로 변환 |
| SLM | Stroke level modification |  이미지의 선명도와 선의 굵기를 수정하여 이미지의 외형을 변화시키는 작업|

1.Abstract<br>
  -stroke guidance maps를 통해서 수정하고자하는 영역을 지정
  
  -모든 픽셀을 고려해서 하는 작업이 아니다 텍스트가 있는 부분에 집중
  
  -합성이미지와 실제사진의 학습을 위해 반지도 학습을 진행
  
2.Introduction

  -목적 : 백그라운드 이미지와 기존의 텍스처와 텍스트 스타일을 유지하면서 텍스트를 수정
  
  -장점 : 어떠한 단어라도 일관성있게 수정가능하고 이미지내에 모든단어도 가능
