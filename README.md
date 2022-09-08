# [DACON] SW중심대학 공동 AI 경진대회 ❮예선❯
###### 2022.08.01~2022.08.26 https://dacon.io/competitions/open/235902/overview/description
## 목차
#### 1. 프로젝트 목표
#### 2. 데이터
#### 3. 데이터 전처리
#### 4. 모델 학습
#### 5. 최종 결과
## 프로젝트 목표
##### 심리학 테스트 데이터를 분석하여 "심리 성향을 예측"하는 알고리즘을 개발하는 것이 대회의 목적이다. (대회 설명란 참고)
## 데이터
<img src = "https://user-images.githubusercontent.com/88043302/189008469-81f0fbe9-def2-41c7-83dc-6e70f4fd794f.png" width="30%" height="30%">

##### 대회 측에서 제공하는 데이터는 train data 14999개, test data 35451개, sample_submission.csv(제출할 양식이 담긴 csv파일)이다. train data에는 target인 nerdiness값과,  68개의 질문에 해당하는 각 응답들이 담겨있으며, test data에는 nerdiness를 제외한 나머지 응답값들이 담겨 있다. 

## 데이터 전처리  
##### 1. 인덱스 행 제거 : 인덱스 행은 학습에 도움이 되지 않는 불필요한 데이터이기 때문에 train, test data에서 각각 삭제해주었다.
##### 2. 이상치 제거: IQR 이상치 탐지 방법으로 introelapse, testelapse, surveyelapse의 이상치 탐지 및 제거를 수행하였다. 또한, familysize와 age에도 이상치가 존재하므로, familysize는 11명 미만인 데이터만 남기고, age는 120살 미만인 데이터만 남기는 방식으로 이상치를 제거하였다. 
##### 3. null data 채우기: Q1~26에 해당하는 응답이 모두 비어있는 8개의 행은 아예 삭제하고, 나머지 null data는 해당 열의 평균을 반올림하여 정수값으로 만들어줘서 채웠다. 
##### 4. age 범주화: 연속형 변수인 age를 10살 단위로 범주화하였다. 
##### 5. 덜 중요하다고 판단되는 feature 열 삭제: urban, gender, voted, ASD, married, hand, engnat, VCL1~16의 특성 중요도가 낮다고 판단하여 모델 성능을 높이기 위해 해당 열을 삭제하였다.  

## 모델 학습
##### 1. 전처리 과정을 마친 train data를 sklearn의 train_test_split 를 통해 train, test data로 랜덤하게 나눠준다. 
##### 2. compare_models 함수를 통해 정확도(Accuracy)를 기준으로 앙상블시킬 상위 3개의 모델을 선정하였다. 그 결과 Random Forest Classifier, Extra Trees Classifier, Light Gradient Boosting Machine(LGBM)이 선정되었고, 성능 향상을 위해 각각의 모델을 tune_model함수를 통해 튜닝을 시켜주었다. 
##### 3. 튜닝 완료한 3개의 모델 블랜딩을 진행하고, 최종 모델 학습을 진행하였다. 

## 최종 결과
##### final_test_df
