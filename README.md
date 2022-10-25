# 스마트 팩토리 APS 시스템, 재고관리 시스템 구축

<img src = "https://user-images.githubusercontent.com/96767467/168734234-0495ba1f-bc34-441f-8608-a26c13b8b066.png" align = 'center' width = "95%" >

### 들어가기 전에
- 발표영상: https://www.youtube.com/watch?v=U2zhgZl9Igc  
- 콘크리트 혼화제 회사에서 제공받은 데이터를 활용하여 APS 시스템 구축 및 업무 효율 향상 프로젝트입니다.  
- 한정된 데이터만 제공받아 데이터를 증식하여 활용하였습니다. (시계열분석과 변수를 적용하여 증식)  
- 자세한 내용은 소스코드 혹은 ppt를 참고해 주시기 바라며 모바일은 최적화가 되어 있지 않습니다.  
</br>

### 목차

[1. 기획 및 과제 정의](#1-기획)</br>
[2. 모델링](#2-모델링)</br>
[3. 시각화 및 기대효과](#3-활용방안-및-기대효과)</br>
</br>

---

## 1. 기획

### 1-1 비즈니스 이슈
<p align = 'center'>
<img src = "https://user-images.githubusercontent.com/96767467/197486303-3a25f9da-5093-4ff0-8733-60b17f7be90e.png" width = "95%" height = "53%">
    </p>
<P>
이번 과제를 받은 회사는 콘크리트 혼화제 유통 회사로, 생산계획 및 원자재 발주 부분에서 대부분의 문제들이 발생하고 있었습니다.
    </p>
</br>
<p>
<img src = "https://user-images.githubusercontent.com/96767467/197487193-9b81e875-98af-4384-bbae-615861b92136.png" width = "95%" height = "600">
    </p>
<p> 
위와 같은 메인 이슈들을 확인하였고, APS시스템을 도입하는 솔루션을 기획하였습니다.
    </p>
</br>

### 1-2 과제 정의

<p align = 'center'>
<img src = "https://user-images.githubusercontent.com/96767467/197488259-6e4c666d-1cd4-467d-9d62-1242988276a9.PNG" width = "48%" height = "300">
<img src = "https://user-images.githubusercontent.com/96767467/197488578-5b0f5902-d6a5-4a42-910d-ac2f6719974d.PNG" width = "48%" height = "300">
</p>
<p>
APS시스템은 수요 예측을 통해 생산계획 및 자원소요계획에 대해 의사결정을 지원해주는 시스템입니다.  
감에 의존한 발주와 매일 수동으로 진행되는 재고 관리를 이 시스템을 이용하여 자동으로, 신뢰도 있게 처리하는 것을 목표로 합니다.  
</p>
</br>
</br>
    
## 2. 모델링

### 2-1 시스템 요약도

<p align = 'center'>
<img src = "https://user-images.githubusercontent.com/96767467/197506297-cc7ae876-6c5f-4fda-b524-b24285bd2f32.PNG" align = 'center' width = "95%" height = "600">
</p>

</br>

### 2-2 데이터 마트 구축

<p align = 'center'>
<img src = "https://user-images.githubusercontent.com/96767467/197506479-b9f0e7b5-f754-4507-981e-612df49386a1.PNG" width = "48%" height = "300">
<img src = "https://user-images.githubusercontent.com/96767467/197506489-fbf59ebd-7625-44de-af33-1de731e579d9.PNG" width = "48%" height = "300">
</p>
<p>
통일성 없이 흩어진 데이터를 오류 수정 후 통합하여 가공합니다.  
이 때 기상청, 건축 착공면적 등의 필요한 외부 데이터도 함께 업데이트 됩니다.
</p>
</br>
   
### 2-3 변수 정의

<p align = 'center'>
<img src = "https://user-images.githubusercontent.com/96767467/197507093-d132e538-ce06-4a38-be03-ead976ea0c5f.PNG" align = 'center' width = "95%" height = "600">
</p>
<p>
수요 예측은 위와 같은 형태로 진행됩니다.  
판매량만을 가지고 예측하면 신뢰도가 떨어지므로, 판매량과 관련있는 외부 변수를 정의하고 시계열 모델을 통해 외부 변수를 예측한 뒤 회귀모델을 통해 다시 판매량을 예측합니다.
    </p>
<p align = 'center'>
<img src = "https://user-images.githubusercontent.com/96767467/197507707-5e067701-4409-4568-b6ad-84cf0bc3cdf5.PNG" width = "48%" height = "300">
<img src = "https://user-images.githubusercontent.com/96767467/197507717-8ecf503b-558a-4f6d-af57-4c46e5818e9b.PNG" width = "48%" height = "300">
</p>
<p>
도메인 지식에 따라 온도, 습도, 강수량, 신적설량이 변수로 선정되었습니다.</br>
추가적으로 건설경기 동행지표인 건축 착공면적을 변수로 선정하였습니다.</br>
</p>
</br>

### 2-4 모델 선택

<p align = 'center'>
<img src = "https://user-images.githubusercontent.com/96767467/197508593-0a34f69b-575d-4c4f-a606-2a310256fd5b.PNG" align = 'center' width = "95%" height = "600">
<p>
외부 변수들을 시계열 모델인 Auto ARIMA와 GRU모델을 통해 증강하고 검증한 R2 스코어는 위와 같고, Auto ARIMA 모델을 최종 선택 하였습니다.
    </p>
</p>
<p align = 'center'>
<img src = "https://user-images.githubusercontent.com/96767467/197508911-253c7ae4-07e6-4aa4-87fe-a538f9e914a7.PNG" align = 'center' width = "95%" height = "600">
</p>
<p>
이제 회귀 모델을 사용하여 판매량을 예측, 검증한 결과입니다.
최종적으로 LGBM 모델을 사용하였고, 감에 의존하던 발주의 오차보다 2배 이상 개선되었습니다.
</p>

</br>
</br>
---- 작성중 ----
