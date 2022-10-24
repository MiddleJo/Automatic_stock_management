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
[3. 시각화 및 기대효과](#3-군집화-및-인사이트-도출)</br>
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
<p>
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
<img src = "https://user-images.githubusercontent.com/96767467/175251069-166e03e6-91cf-4766-82ee-c5c8e105e20d.png" align = 'center' width = "95%" height = "600">
</p>
</br>

### 2-2 변수 개발

<p align = 'center'>
<img src = "https://user-images.githubusercontent.com/96767467/175251614-564230f4-f2c4-4a24-a3c0-f4984e43cc26.png" width = "48%" height = "300">
<img src = "https://user-images.githubusercontent.com/96767467/175251678-1b1fa22d-4beb-4025-9a4e-37f3b83ba8e9.png" width = "48%" height = "300">
</p>
<p>
제휴사별 카테고리를 통합 카테고리로 재분류하고, 변수를 개발합니다.  
변수는 다중공선성 문제가 없도록 상관도를 고려하여 재선정하고, 오른쪽 사진과 같이 구성되었습니다.  
</p>
</br>
   
### 2-3 모델 선정

<p align = 'center'>
<img src = "https://user-images.githubusercontent.com/96767467/175253738-61d12605-e7f5-491e-a692-d41f083f91cf.png" width = "48%" height = "300">
<img src = "https://user-images.githubusercontent.com/96767467/175253773-0c1557e9-b174-4f74-b0a7-a2c51eed4a1e.png" width = "48%" height = "300">
</p>
<p>
유의 고객을 예측하는 분류 모델을 선정하기 위해 평가지표들을 확인합니다.</br>
2년 중 7분기분을 train 데이터와 validation 데이터로 나누어 모델을 구성하였고, 8분기를 test 데이터로 활용하였습니다.</br>
오른쪽 사진 내용과 같이 XGBoost를 선택하였습니다.</br>
</p>

</br>
</br>
   
## 3. 군집화 및 인사이트 도출

### 3-1 군집 개수 선정

<img src = "https://user-images.githubusercontent.com/96767467/175255644-5c000aba-0193-40a1-a7b1-246cebc00fb3.png" align = 'center' width = "95%" height = "600">
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
