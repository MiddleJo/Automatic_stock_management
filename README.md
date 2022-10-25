# 스마트 팩토리 APS 시스템, 재고관리 시스템 구축

<img src = "https://user-images.githubusercontent.com/96767467/168734234-0495ba1f-bc34-441f-8608-a26c13b8b066.png" align = 'center' width = "95%" height = "53%">

### 들어가기 전에
- 발표영상: https://www.youtube.com/watch?v=U2zhgZl9Igc  
- 콘크리트 혼화제 회사에서 제공받은 데이터를 활용하여 APS 시스템 구축 및 업무 효율 향상 프로젝트입니다.  
- 한정된 데이터만 제공받아 데이터를 증식하여 활용하였습니다. (시계열분석과 변수를 적용하여 증식)  
- 자세한 내용은 [소스코드](https://github.com/MiddleJo/Automatic_stock_management/tree/main/code) 혹은 pdf를 참고해 주시기 바랍니다.
- 마지막 Tableau 데모 부분은 영상 참고해주시면 감사하겠습니다. 
</br>

### 목차

[1. 기획 및 과제 정의](#1-기획)</br>
[2. 모델링](#2-모델링)</br>
[3. 활용방안 및 기대효과](#3-활용방안-및-기대효과)</br>
[4. 데모 시연](#4-데모-시연)</br>
[팀원 소개](#팀원-소개)</br>
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
<img src = "https://user-images.githubusercontent.com/96767467/197487193-9b81e875-98af-4384-bbae-615861b92136.png" width = "95%" height = "53%">
    </p>
<p> 
위와 같은 메인 이슈들을 확인하였고, APS시스템을 도입하는 솔루션을 기획하였습니다.
    </p>
</br>

### 1-2 과제 정의

<p align = 'center'>
<img src = "https://user-images.githubusercontent.com/96767467/197488259-6e4c666d-1cd4-467d-9d62-1242988276a9.PNG" width = "48%" height = "27%">
<img src = "https://user-images.githubusercontent.com/96767467/197488578-5b0f5902-d6a5-4a42-910d-ac2f6719974d.PNG" width = "48%" height = "27%">
</p>
<p>
APS시스템은 수요 예측을 통해 생산계획 및 자원소요계획에 대해 의사결정을 지원해주는 시스템입니다.</br>
감에 의존한 발주와 매일 수동으로 진행되는 재고 관리를 이 시스템을 이용하여 자동으로, 신뢰도 있게 처리하는 것을 목표로 합니다.  
</p>
</br>
</br>
    
## 2. 모델링

### 2-1 시스템 요약도

<p align = 'center'>
<img src = "https://user-images.githubusercontent.com/96767467/197506297-cc7ae876-6c5f-4fda-b524-b24285bd2f32.PNG" align = 'center' width = "95%" height = "53%">
</p>

</br>

### 2-2 데이터 마트 구축

<p align = 'center'>
<img src = "https://user-images.githubusercontent.com/96767467/197506479-b9f0e7b5-f754-4507-981e-612df49386a1.PNG" width = "48%" height = "27%">
<img src = "https://user-images.githubusercontent.com/96767467/197506489-fbf59ebd-7625-44de-af33-1de731e579d9.PNG" width = "48%" height = "27%">
</p>
<p>
통일성 없이 흩어진 데이터를 오류 수정 후 통합하여 가공합니다.  
이 때 기상청, 건축 착공면적 등의 필요한 외부 데이터도 함께 업데이트 됩니다.
</p>
</br>
   
### 2-3 변수 정의

<p align = 'center'>
<img src = "https://user-images.githubusercontent.com/96767467/197507093-d132e538-ce06-4a38-be03-ead976ea0c5f.PNG" align = 'center' width = "95%" height = "53%">
</p>
<p>
수요 예측은 위와 같은 형태로 진행됩니다.  
판매량만을 가지고 예측하면 신뢰도가 떨어지므로, 판매량과 관련있는 외부 변수를 정의하고 시계열 모델을 통해 외부 변수를 예측한 뒤 회귀모델을 통해 다시 판매량을 예측합니다.
    </p>
</br>
<p align = 'center'>
<img src = "https://user-images.githubusercontent.com/96767467/197507707-5e067701-4409-4568-b6ad-84cf0bc3cdf5.PNG" width = "48%" height = "27%">
<img src = "https://user-images.githubusercontent.com/96767467/197507717-8ecf503b-558a-4f6d-af57-4c46e5818e9b.PNG" width = "48%" height = "27%">
</p>
<p>
도메인 지식에 따라 온도, 습도, 강수량, 신적설량이 변수로 선정되었습니다.</br>
추가적으로 건설경기 동행지표인 건축 착공면적을 변수로 선정하였습니다.</br>
</p>
</br>

### 2-4 모델 선택

<p align = 'center'>
<img src = "https://user-images.githubusercontent.com/96767467/197508593-0a34f69b-575d-4c4f-a606-2a310256fd5b.PNG" align = 'center' width = "95%" height = "53%">
</p>
<p>
외부 변수들을 시계열 모델인 Auto ARIMA와 GRU모델을 통해 증강하고 검증한 R2 스코어는 위와 같고, Auto ARIMA 모델을 최종 선택 하였습니다.
    </p>
</br>
<p align = 'center'>
<img src = "https://user-images.githubusercontent.com/96767467/197508911-253c7ae4-07e6-4aa4-87fe-a538f9e914a7.PNG" align = 'center' width = "95%" height = "53%">
</p>
<p>
이제 회귀 모델을 사용하여 판매량을 예측, 검증한 결과입니다.
최종적으로 LGBM 모델을 사용하였고, 감에 의존하던 발주의 오차보다 2배 이상 개선되었습니다.
</p>

</br>
</br>
   
## 3. 활용방안 및 기대효과

### 3-1 생산 계획

<p align = 'center'>
<img src = "https://user-images.githubusercontent.com/96767467/197734915-b4ea6342-34fe-4799-8501-4974a98d44f5.PNG" align = 'center' width = "95%" height = "53%">
</p>
<p>
저희의 솔루션에서 의사결정 지원은 2가지 방향성이 있습니다.</br>
- 6개월 정도의 비교적 먼 미래의 투자계획</br>
- 3개월 정도의 비교적 가까운 미래의 생산 계획</br>
이 기간을 고려하여 각각 수요 예측을 진행할 것입니다.
</p>
</br>

### 3-2 재고관리 및 발주 자동화

<p align = 'center'>
<img src = "https://user-images.githubusercontent.com/96767467/197737004-3f0d4a4d-ea0d-4770-a59a-4467fed72539.PNG" width = "48%" height = "27%">
<img src = "https://user-images.githubusercontent.com/96767467/197737033-28e221e7-cdc9-4c45-8cc2-a436086eec52.PNG" width = "48%" height = "27%">
</p>
<p>
재고관리는 리드타임과 안전 재고량을 고려하여야 합니다.</br>
리드타임은 도메인 정보를, 안전 재고량은 오른쪽과 같이 정의하였습니다.</br>
</p>
</br>

<p align = 'center'>
<img src = "https://user-images.githubusercontent.com/96767467/197734915-b4ea6342-34fe-4799-8501-4974a98d44f5.PNG" align = 'center' width = "90%" height = "50%">
</p>
<p>
위 두가지를 고려하여 생산관리 시스템(MES)가 자동으로 발주량을 산정해줍니다.
</p>
</br>


### 3-3 기대효과

<p>
<img src = "https://user-images.githubusercontent.com/96767467/197737971-35588c52-cb1e-41ee-ac6b-dc9cde13bcef.PNG" align = 'center' width = 95% height = "53%">
</p>
<p>
시스템을 도입하면 재고관리, 고객관리, 생산계획 수립에 걸리는 시간을 최소 1시간 30분가량 절약할 수 있습니다.</br>
추가적으로 회의나 개인 업무시 시각화 도구를 통해 업무 효율이 비약적으로 상승합니다.
</p>
</br>
</br>

## 4. 데모 시연
![시연영상_16배속](https://user-images.githubusercontent.com/96767467/197757181-e0b51e02-8fc9-457e-87f7-20e905818d45.gif)

<p>
<img src = "https://user-images.githubusercontent.com/96767467/197756850-bacdd86f-82f5-4bb2-8912-b80c2595bc51.gif" align = 'center' width = 95% height = "53%">
</p>
<p>
시연 영상을 16배속으로 간단히 보여드립니다.</br>
자세한 사항이 궁금하시면 [발표영상](https://www.youtube.com/watch?v=U2zhgZl9Igc) 참고해 주세요.
</p>
</br>
</br>

## 팀원 소개
----
<img align="left" width="180" height="180" src="https://user-images.githubusercontent.com/96767467/175253262-f4614359-abd2-4839-b8d3-0f7b8dd32f53.jpg" />

- 조남현(CHO NAM HYEON)</br>
- 변수 개발, 시계열 모델</br></br></br>
MAIL : chonh0531@gmail.com </br>
Github : https://github.com/MiddleJo </br>

----
<img align="left" width="180" height="180"  src="https://user-images.githubusercontent.com/102858692/161480452-fc8d952a-b964-4b44-8a9b-b5eab3652f89.png"/>

- 박광민(PARK KWAMG MIN)</br>
- DB구축, 발주 자동화</br></br></br>
MAIL : qkrrhk@gmail.com </br>
Github : https://github.com/KMP94</br>

----
<img align="left" width="180" height="180" src="https://user-images.githubusercontent.com/102858692/161481002-6c4f9f96-5ae6-4ea6-b2d0-d0a665b158fa.png"/>

- 김기현(KIM GI HYUN)</br>
- 변수 개발, 시계열 모델</br></br></br>
MAIL : luckyboy3214@naver.com </br>
Github : https://github.com/spiner321</br>

----
<img align="left" width="180" height="180" src="https://user-images.githubusercontent.com/102858692/161481162-740c39fb-38d5-469c-9227-21fa9f0c6925.png" />

- 마경수(MAH KYUNG SOO)</br>
- DB구축, 사용설명서</br></br></br>
MAIL : kal198309@hanmail.net </br>
Github : https://github.com/EDPS-7532</br>

----

<img align="left" width="180" height="180" src="https://user-images.githubusercontent.com/102858692/161481240-b1c40cae-21bb-41b4-922d-d34857eaaf58.png"/>

- 문성윤(MOON SEONG YUN)</br>
- 시계열모델, PPT 제작</br></br></br>
MAIL : msy7367@gmail.com </br>
Github : https://github.com/Syoon0710</br>

----

<img align="left" width="180" height="180" src="https://user-images.githubusercontent.com/102858692/161481322-ec6afce7-e8b6-4355-9680-0526d9df6b21.png" />

- 최종원(CHOI JONG WON)</br>
- 시각화, 프레젠테이션</br></br></br>
MAIL : joanna.jongwon.choi@gmail.com </br>
Github : https://github.com/joannajongwonchoi</br>

----
