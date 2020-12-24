# L.Point 추천 시스템

진행 정보: 완료, 팀

### **왕밤빵 팀 구성**


| 이름   |  구성   |                      역할                  |
| :----: | :----: |  :---------------------------------------: | 
| 박기민   |  팀장   | 데이터 정제 및 생성, User2Vec 모델 구현   | 
| 이상운  |  팀원   | Wide & Deep 모델 구현, 외부 데이터 수집   | 
| 차나래  |  팀원   | 평가지표 개발 및 성능 개선, 결과 시각화  | 
| 장선우  |  팀원   | 데이터 EDA 및 데이터 분석, PPT 작성 | 

<br>

### **작업 환경**

- Ubuntu 18.04
- Intel i7-10875H
- GeForce RTX 2070 / RAM 8GB
- Jupyter notebook

### **Skills**

- tensorflow 2.3.0
- python
- Crawling
- pandas


## 🗒️ Content

---

### □ **데이터 정보**

총 4개의 데이터를 받았습니다. 

![image/Untitled.png](image/Untitled.png)

- **온라인 행동 정보** : 업종별로 고객들의 온라인 행동 정보
- **거래 정보**  : 온,오프라인 업종별로 고객들의 구매 정보
- **고객 Demographic 정보** : 고객들의 id와 성별, 연령대 정보
- **상품 분류 정보** : 상품 정보 (대분류, 중분류, 소분류 등)

### □ **프로젝트 진행 요약**


| 기간   |                     활동                   |   장소  |
| :----: | :--------------------------------------- |  :------: | 
| 1주차	 |  프로젝트 기획 및 목표 선정, 워크플로우 논의   | 오프라인   | 
| 2주차  |  Raw Data 분석, 추천 시스템 모델 논의   | 오프라인| 
| 3주차  |   추천 시스템 논문 Review, 고객 유형 분석  | 오프라인  | 
| 4주차  | 애자일소다 Kick-off 미팅, 추천 알고리즘 모델 생성 논의|온라인| 
| 5주차  |   LightGBM 기반 Baseline 작성, Feature Selection 논의  | 온라인 | 
| 6주차  |  추천시스템 평가지표 학습, EDA 추가 수행   | 온라인 | 
| 7주차  |  모델 평가 방식 논의, User2Vec 논의   | 온라인 | 
| 8주차  |  Wide&Deep 모델 수행, 데이터 엔지니어링   | 온라인 | 
| 9주차  |   결과 시각화, 모델 개선과 실험  | 온라인 | 
| 10주차 |   PPT 작성, 최종 발표  | 온라인 | 


<br>

## 📒 진행 과정

---

## Overview

### □ Purpose

- 고객의 행동 데이터를 기반으로 상위 K개 제품을 추천.

![image/Untitled%201.png](image/Untitled%201.png)

### □ Structure

![image/Untitled%202.png](image/Untitled%202.png)

- Input 데이터 형식 : Raw 데이터 + 생성 데이터 + 임베딩 데이터 + 외부 데이터
- Model 사용 : Wide & Deep Model
- 평가지표 : MAP, Entropy-Diversity

## Process

### □ [EDA](https://github.com/PEBpung/Lpoint-Hackathon/blob/master/src/02.%EB%8D%B0%EC%9D%B4%ED%84%B0_EDA.ipynb)

제공된 데이터를 이해하기 위해서 EDA를 진행했습니다. 다양한 파일을 EDA했지만, 대표적인 예시를 보여드리겠습니다. 

- 온라인 행동 유형 시각화

    ![image/Untitled%203.png](image/Untitled%203.png)

    - 결제 시도와 구매 완료의 차이가 많이남.
    - 분석 결과 결제 시도 과정에서 중복이 많음.

- 일자별 활동량 추이

    ![image/Untitled%204.png](image/Untitled%204.png)

    - 일자 별로 활동량을 그래프로 나타냄.
    - 분석 결과 요일 별로 특정 패턴을 가지고 있음.
    - 2019-09-15 부근의 과도한 하락은 추석 명절 때문이라고 판단.
    - 추가 분석을 위해서 외부데이터(기상청) 사용

- 고객 분포

    ![image/Untitled%205.png](image/Untitled%205.png)

    - 고객의 성별은 여성이 가장 많고, 나이는 30~40대가 대다수를 차지.
    - 성별과 나이로 고객 타입을 분류한 후 분석한 결과 구매 품목 차이를 보임.

### □ [Keyword Crawling](https://github.com/PEBpung/Lpoint-Hackathon/blob/master/src/00.Sech_kwd_%EB%A7%A4%ED%95%91.ipynb)

기존의 키워드명은 특정 상품을 설명하기에 어려운 단점이 있었습니다. 그래서 Item 데이터에 있는 명칭과 매칭하기 위해 크롤링을 진행했습니다. 

![image/Untitled%206.png](image/Untitled%206.png)

- 검색 행동과 상품의 직접적인 연관성을 파악하기 위해서 크롤링 진행.
- 크롤링을 통해 키워드를 Item 데이터에 있는 분류명으로 변경.

### □ [Data Generator](https://github.com/PEBpung/Lpoint-Hackathon/blob/master/src/04.feature_%EC%83%9D%EC%84%B1.ipynb)

앞서 진행한 EDA에서 얻은 가설을 바탕으로 유의미한 변수를 생성하는 과정을 진행했습니다. 여기서는 생성된 변수의 근거는 생략하겠습니다. 

1. 누적 행동 데이터 생성

    ![image/Untitled%207.png](image/Untitled%207.png){: width="50%" height="50%"}

    - 기존 데이터는 고객의 과거 행동 이력을 반영하고 있지 않음.
    - 행동 변수를 dummy화 시킨 후, 동일 Session 내에서 특정 행동의 빈도를 누적 시켜줌.

2. 최근 행동 데이터

    ![image/Untitled%208.png](image/Untitled%208.png)

    - 최근 행동 데이터에는 고객이 최근 활동한 시간과 서핑 속도를 반영해줌.
    - 그 결과 고객의 행동 패턴을 더 잘 표현 할 수 있었음.

3. 선호 채널과 기기

    ![image/Untitled%209.png](image/Untitled%209.png)

    - 시각화 결과 같은 기기라도 유입채널이 다르면 다른 분포를 가지고 있음.
    - 두 변수를 묶어줌으로 선호하는 경로를 데이터에 반영해줌.

4. 외부 데이터 (기상청)  

    ![image/Untitled%2010.png](image/Untitled%2010.png)

    - EDA 결과에서 날짜 별로 일정 패턴이 존재함을 확인.
    - 좀 더 세밀한 분석을 위해 기상 데이터를 활용.

### □ [User2Vec](https://github.com/PEBpung/Lpoint-Hackathon/blob/master/src/05.User2Vec_%ED%95%99%EC%8A%B5.ipynb)

User가 같이 구매한 Item의 특징을 분석하면 같은 품목을 구매한 다른 User와 상관관계를 분석할 수 있다고 판단하였습니다. 

![image/Untitled%2011.png](image/Untitled%2011.png)

- Row별로 한개 씩 있는 제품을 고객과 세션을 기준으로 장바구니로 묶어줌.
- Cosine Similarity로 평가한 결과 유사한 상품 구매의 경우 벡터의 위치가 가까움.

### □ [Feature Selection](https://github.com/PEBpung/Lpoint-Hackathon/blob/master/src/04.feature_%EC%83%9D%EC%84%B1.ipynb)

Feature engineering과 User2Vec을 마친 후 변수의 중요도를 시각화 해보았습니다. base model인 LGBM 모델을 돌려서 성능 저하가 발생하면 feature을 제거하는 방법을 선택했습니다. 

![image/Untitled%2012.png](image/Untitled%2012.png)

- 중요도 파악 결과 Positive 변수와 Negative 변수로 나눔.
- Negative 변수는 추후 모델을 돌릴 때 제거시킴.

### □ [Model](https://github.com/PEBpung/Lpoint-Hackathon/blob/master/src/07.Wide%26Deep_%EB%AA%A8%EB%8D%B8_%ED%95%99%EC%8A%B5-all.ipynb)

저희는 Wide model과 Deep Model이 결합된 형태인 Wide & Deep 모델을 사용했습니다. Implicit 변수가 많은 Lpoin 데이터의 추천시스템으로 적합하다고 판단했습니다. 

![image/Untitled%2013.png](image/Untitled%2013.png)

- Wide 모델의 경우 Categorical Feature에 대하여 Cross Product 수행하여 새로운 Feature 생성.
- Deep 모델의 경우 Continuous Feature와 Categorical Feature를 Embedding 하여 복합 적용.

### □ [Metric](https://github.com/PEBpung/Lpoint-Hackathon/blob/master/src/07.Wide%26Deep_%EB%AA%A8%EB%8D%B8_%ED%95%99%EC%8A%B5-all.ipynb)
저희는 최종적으로 모델을 평가하기 위해서 MAP와 Entropy-Diversity를 사용했습니다. 

- MAP

    ![image/Untitled%2014.png](image/Untitled%2014.png)

    - 사용 이유 : 추천 점수가 높은 순서로 top-k 개의 아이템을 추천할 때, 상위 추천 아이템을 구매한 것에 더 가중치를 주어서 추천 순서를 차등적으로 평가함.
- Entropy-Diversity

    ![image/Untitled%2015.png](image/Untitled%2015.png)

    - 사용 이유 : 모든 사용자에게 비슷한 아이템을 추천하지 않고, 개인별 추천 아이템들이 다양하게 추천하였는지 평가함.

## 🏆 해커톤 결과

---

- Affel에서 총 13명의 팀이 해커톤을 진행했습니다.
- 최종 발표 심사위원은 애자일소다 대표님, 교장선생님, 퍼실리테이터 분들 이었습니다.
- 중간 발표와 최종 발표 합산 결과 **우수상**을 받았습니다.

## ✍️ 느낀 점

---

- Aiffel과정 특성 상 딥러닝에 초점이 맞춰진 교육이었는데, 데이터 분석 프로젝트를 진행하니까 어려운 부분이 많았습니다.
    - 캐글이나 데이콘의 e-commerce 데이터를 다룬 노트북을 참고해서 이해하려고 노력했습니다.
    - 여러가지 파생변수를 만들때 효용성과 필요성을 분석하기 위한 여러가지 방법을 추가적으로 학습했습니다.
- 팀 프로젝트 진행 시 코로나로 인해서 온라인 미팅의 어려움이 있었습니다.  그래서  소통과 스케줄링이 더욱 중요하다는 사실을 깨달았습니다.
    - 각자 진행하는 파트를 공유하기 위해 적절한 예시와 정리로 온라인 의사소통을 원활하게 진행했습니다.
    - 모두 모여서 적절한 파트 분배를 했고, 진행하면서 어려운 부분은 미팅 시간에 화면 공유를 통해 아이디어를 모았습니다.
    - 분업을 할 수 있는 부분에서는 파트를 분배하고, 집중해야 될 부분은 단계를 세워서 진행했습니다.