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
<img src = "https://user-images.githubusercontent.com/96767467/197486303-3a25f9da-5093-4ff0-8733-60b17f7be90e.png" width = "95%" height = "600">
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
   
## 3. 활용방안 및 기대효과

### 3-1 군집 개수 선정

<p align = 'center'>
<img src = "https://user-images.githubusercontent.com/96767467/197507707-5e067701-4409-4568-b6ad-84cf0bc3cdf5.PNG" width = "48%" height = "300">
<img src = "https://user-images.githubusercontent.com/96767467/197507717-8ecf503b-558a-4f6d-af57-4c46e5818e9b.PNG" width = "48%" height = "300">
</p>
</br>

<p>
이제 유의고객이 될 고객을 예측하고, 해당 고객들을 군집분석하여 군집별 솔루션을 제시합니다.</br>
군집화 알고리즘은 K-평균 알고리즘을 사용하였으며, 실루엣 계수를 확인하고 군집 수는 3개로 결정합니다.</br>
</p>
</br>

### 3-2 군집별 솔루션

<p align = 'left'>
<img src = "https://user-images.githubusercontent.com/96767467/175773504-a6bb28bc-4e30-457b-b48a-34f9fe4e4501.png" width = "70%" height = '450'>
</p>

"잠재적 충성 고객층"</br>  
- 단계별 마케팅</br>
- 평소 선호 제품군 추천</br> 
- 소속감을 부여하는 서비스</br>
</br>

<p align = 'left'>
    <img src = "https://user-images.githubusercontent.com/96767467/175773510-31826c42-7a54-4e97-8df7-ccff5e17c733.png" width = '70%' height = '450'>
    <center width = '28%' height = '450'>
    "이탈 예상 고객층"</br>  
    - 이벤트 등의 추가 서비스</br>
    - 자주 구매하는 상품 위주</br>
    - 온라인 채널 홍보</br>
    </center>
</p>
</br>

<p align = 'left'>
    <img src = "https://user-images.githubusercontent.com/96767467/175773514-33ca090d-b050-4e02-9162-9cf0b50d8734.png" width = '68%' height = '450'>
    <p width = '28%' height = '450'>
    "구매력 높은 고객층"</br>  
    - 구매 금액에 따른 혜택</br>  
    - 고품질의 고가 상품군</br>
    - 계절 상품 개발</br>
    </p>
</p>
</br>

### 3-3 개인맞춤형 추천시스템

<p>
<img src = "https://user-images.githubusercontent.com/96767467/175774946-af6d01e7-b3be-4563-922d-5b5a3b58ae0b.png" align = 'center' width = 95% height = "600">
</p>
<p>
최근에는 큰 틀에서의 추천 시스템은 효과를 별로 보지 못합니다.</br>
따라서 군집솔루션과 고객들의 개인 선호도를 결합하여 Base 추천 시스템을 구성할 필요가 있습니다.</br>
여기에 계절 인기상품과 개인 선호 상품군, 가격대 등이 결합되어 더 유의미한 추천이 이루어집니다.
</p>

<p align = 'center'>
<img src = "https://user-images.githubusercontent.com/96767467/175775591-d8929e02-32c5-41cc-bd8e-f13df1ab57c6.png" width = "48%" height = "300">
<img src = "https://user-images.githubusercontent.com/96767467/175775652-584d0a9e-a149-4f92-a215-1f769211a9e0.png" width = "48%" height = "300">
</p>
<p>
Surprise api를 통해 구성하였으며, Baseline은 SVD알고리즘과 Knn 알고리즘 중 rmse가 낮은 것이 선택되어 적용되도록 하였습니다.</br>
전년 동월의 상품군별 인기 상품과 개인 추천 리스트와 곂치는 것이 있다면, 가중치를 주어 상단으로 올라오게 됩니다.</br>
이를 통해 개인 맞춤형 추천에 군집 솔루션을 자연스럽게 녹일 수 있으며, 가중치 등은 전문가와 함께 보완할 여지가 있습니다.
</p>

</br>
</br>

## 팀원 소개

- 시계열 모델을 활용한 수요 예측
- 흩어진 데이터 DB에 통합 및 업데이트 자동화
- Tableau를 통한 시각화 및 사용설명서 제작
