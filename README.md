# [DACON] SW중심대학 공동 AI 경진대회 ❮예선❯
2022.08.01~2022.08.26 

https://dacon.io/competitions/open/235902/overview/description

## Table of Contents
#### 1. Project Description
#### 2. Data
#### 3. Data Preprocessing
#### 4. Models
#### 5. Results
#### 6. Supplementary points

## Project Description
심리학 테스트 데이터를 분석하여 "심리 성향(nerdiness)을 예측"하는 알고리즘을 개발하는 것이 대회의 목적이다. 예측해야하는 값인 nerdiness는 0 또는 1의 값을 가지므로, classification과 관련한 문제이다. 따라서 본 팀은 Google Colab에서 Python으로 PyCaret의 분류알고리즘을 활용하여 nerdiness 예측을 진행하였다. 


## Data
<img src = "https://user-images.githubusercontent.com/88043302/189008469-81f0fbe9-def2-41c7-83dc-6e70f4fd794f.png" width="30%" height="30%">

대회 측에서 제공하는 데이터는 train data 14999개, test data 35451개, sample_submission.csv(제출할 양식이 담긴 csv파일)이다. train data에는 target인 nerdiness값과,  68개의 질문에 해당하는 각 응답값들이 담겨있으며, test data에는 nerdiness를 제외한 나머지 68개의 질문에 해당하는 응답값들이 담겨 있다.

train data는 모델 학습에 활용하였고, finalized된 최종 모델에 test data를 넣어 nerdiness값을 예측하고, 예측된 nerdiness값들이 담긴 최종 submission.csv를 제출하였다. 

## Data Preprocessing  
- 인덱스 행 제거 
  - 불필요한 인덱스 행을 train, test data에서 각각 삭제해주었다.

- 이상치 제거
  - IQR 이상치 탐지: introelapse, testelapse, surveyelapse의 이상치 탐지 및 제거를 수행하였다.
  - familysize와 age에 이상치가 존재하지만, IQR로 해결되지 않았다. 따라서, familysize는 11명 미만인 데이터만 남기고, age는 120살 미만인 데이터만 남기는 방식으로 이상치를 제거하였다. 
- null data 채우기
  - Q1~26에 해당하는 응답이 모두 비어있는 8개의 행은 전부 삭제하였다.
  - 나머지 null data는 해당 열의 평균을 반올림하여 채웠다. 
- age 범주화
  - 연속형 변수인 age를 10살 단위로 범주화하였다. 
- 몇몇 feature 열 삭제
  - urban, gender, voted, ASD, married, hand, engnat, VCL1~16의 특성 중요도가 낮다고 판단하여 모델 성능을 높이기 위해 해당 열을 삭제하였다.  

## Models
***PyCaret의 compare_models()메소드를 이용해 모델들을 비교하고, tune_model()메소드를 통해 각 모델별로 튜닝을 실시한 뒤 blend_models()메소드를 통해 모델 블랜딩을 수행하였다.*** 
1. 전처리 과정을 마친 train data를 sklearn의 train_test_split 를 통해 train, test data로 랜덤하게 나눠주었다. 
2. compare_models 함수를 통해 정확도(Accuracy)를 기준으로 앙상블시킬 상위 3개의 모델을 선정하였다. 그 결과 Random Forest Classifier, Extra Trees Classifier, Light Gradient Boosting Machine(LGBM)이 선정되었고, 성능 향상을 위해 각각의 모델을 tune_model함수를 통해 튜닝을 시켜주었다. 
<img src = "https://user-images.githubusercontent.com/88043302/189058191-50ef8206-4f16-40a3-bdcd-d52c01b908e3.png" width = "60%" height ="40%">
3. 튜닝을 완료한 3개의 모델을 블랜딩하고, 최종 모델 학습을 진행하였다. 
<img src = "https://user-images.githubusercontent.com/88043302/189058596-ec328dc8-ace9-46fe-98bf-60bc2965c0ed.png" width="40%" height="40%">
<img src = "https://user-images.githubusercontent.com/88043302/189058829-4009fd72-afd8-410d-949b-95c7be83861b.png" width="60%" height="40%">

## Results
test data를 finalized된 최종 모델에 넣고 nerdiness 예측을 진행하였을 때, 다음과 같이 예측 결과가 도출되었다. 

<img src = "https://user-images.githubusercontent.com/88043302/189061753-6fbd0475-ad2b-4a18-ba4a-91bf32b7230c.png" width="40%" height="30%">

대회에 최종 제출한 결과, public score 0.80399, private score 0.79721을 기록하였다.

#### Supplementary points (이 내용은 우리끼리만 공유(나중에 삭제할 것임), 추가할 거 있으면 알아서 추가하기) 
데이터 전처리에서 q_sum 포함못시켜본게 아쉽(코드를 잘못 옮긴건지 sum이 이상하게 출력되어서 시간관계상 뺐음 ㅠㅠ내가 제대로 넣었다면 정확도 더 높게 나왔을듯...!! )
introelapse, surveyelapse, testelapse를 scaling 해보았다면..?? 
시간관계상 모델 학습시에 파라미터 충분히 변경해보지 못한 것 (폴드 수나 반복횟수 등)
