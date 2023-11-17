<img width="1000" alt="header" src="https://user-images.githubusercontent.com/134662856/283848502-77963282-3ee1-41bb-8569-08cc4863c45a.png">
<!--이미지 내에 타이틀 내용 기입할 것,(이미지만 보고 무슨 내용인지 직관적으로 알수있게끔) -->

<br>

# Project title
### 5가지 운동 종류의 정확한 자세를 추론하고 카운팅을 지원하는 운동 인식 인공지능 모델

<br>

# Description
### 앱 기능
- 운동 영상을 기반으로 운동의 종류를 판단하고, 해당 운동 동작에 대한 정확도를 카운트마다 평가.
- 정확도 추론 모델에 따라 개수를 카운팅 해주는 인공지능 코칭 서비스를 제공함으로써 보다 객관적인 운동 판단이 가능해짐.

### 기술 선택 이유
- 이미지, 비디오 기술 및 사람 동작 인지 연구가 지속적으로 성장하나 헬스산업 분야에서 
적용이 미비함.
- 2023년 9월 기준, 상용화된 운동 관련 앱들의 AI 적용 서비스 부족 현황을 개선함과 동시에 기존 운동 AI 서비스와 비교하여 아래와 같은 차별점을 둠.
- 총 5가지의 운동을 지원하며, 여러 각도 데이터 활용 및 절대지표값을 학습에 사용함으로써 다각도에서 이용 가능한 서비스 개발.

### 미래 발전 방향
- 수집한 데이터셋의 다양한 운동을 추가하여 서비스의 운동 다양성을 확장할 계획
- 운동 종류마다 정자세를 판단하는 기준은 3-5가지 존재하며, 현재 checkmate는 3-5가지의 조건들을 하나로 통합하여 정확도 추론 모델을 학습. 따라서, 조건을 개별로 학습 시키면 정확도 향상을 기대할 수 있음.
- 현재의 기술 개발은 영상 업로드만 가능, cam 과 연결하여 실시간 평가가 가능하게 발전시킬 예정


<br>

# Table of contents
[1) Project title](#Project-title) <br>
[2) Description](#Description) <br>
[3) Table of contents](#Table-of-contents) <br>
[4) Project timeline](#Project-timeline) <br>
[5) Service demonstration video](#Service-demonstration-video) <br>
[6) Project process](#Project-process) <br>
[7) Badges](#Badges) <br>

<br>

# Project timeline
23.09.04 ~ 23.10.19 (약 6 주간)
<br>

<img width="1000" alt="timeline" src="https://user-images.githubusercontent.com/134662856/282296372-79a4948a-6998-4d4c-86b9-3e186783c610.png">

<br>

# Service demonstration video
![image](https://user-images.githubusercontent.com/134662856/278372833-7c8a5ccf-e1ce-41ff-b877-a05f6087fcad.gif)

<br>

# Project process
## 1. 데이터 준비
**[폴더] 1_analysis**
1. 데이터 수집

<img width="1000" alt="dataset" src="https://user-images.githubusercontent.com/134662856/283759846-aa3efdda-24ea-4a26-9145-47edd8dfced4.png">

- AI Hub에서 제공한 '피트니스 자세 이미지' 데이터셋을 수집.
- 약 1.04 TB 압축 파일로 하나의 운동 영상을 총 32개의 이미지 형태로 저장되어 있는 원천 데이터와 해당 운동 영상을 레이블링한 정보가 담긴 레이블링 데이터로 구성.
- 이미지 파일은 MediaPipe의 pose를 이용하여 33개의 포인트 x,y,z 좌표를 추출한 후, labeling 파일과 merge하여 모델 학습에 이용되는 데이터셋을 구축. 

<br>

2. 데이터 구조

<img width="1178" alt="dataframe" src="https://user-images.githubusercontent.com/134662856/283839880-5c9c5d11-4eb6-46e1-9f6c-5899772e27e2.png">

- 레이블링 데이터:
  - correct (정확도 추론 모델에 사용): 실제 운동 수행의 정자세를 판단하는 레이블
  - exercise (운동 분류 모델에 사용): 각 운동의 분류를 나타내는 레이블
- 이미지 데이터:
  - 32개의 행이 하나의 운동 영상을 나타내며, 1 sequence를 형성.
  - MediaPipe를 통해 얻은 99개의 좌표값으로 구성
  - 참조: 이미지 파일명을 통해 레이블링 데이터와 연결 및 참조

<br>

## 2. EDA 및 전처리

- 결측치 처리 : mediapipe에서 좌표값이 추출되지 않는 이미지 등의 이유로 결측치가 존재하는 경우, 해당 32개의 이미지를 모두 제거 (이유작성 필요, 충분한 데이터 수를 확보했기 때문에~~~ )
- 분류모델의 성능 평가에서 f1 score 가 상대적으로 낮은 두 운동의 EDA 를 진행.
- mediapipe 를 이용하여 좌표를 뽑아낸 경우 좌표값들이 정규화 되어 나오기 때문에 어느 각도, 이미지의 사이즈 등에서 자유도가 높은 편이다. 그렇지만 

<br>

## 3. 모델링
<img width="1178" alt="data flow of service" src="https://user-images.githubusercontent.com/134662856/279037487-81c602b8-144f-4e6b-a02a-76c3452a6e4b.png">
<!--전체적인 데이터(영상)이 들어오면 어떻게 처리되어 user 에게 보여지는지 한장의 이미지로 설명하기-->
<img width="1178" alt="data flow of service" src="https://user-images.githubusercontent.com/134662856/279430604-939f5f49-b12a-4d8a-a19d-91328ffc6540.png">

### 1. 분류 모델
**[폴더] 2_sBert**

- categorical boost model 
- EDA를 통해 알게된 각 exercise별 분류에 각 주요 부위 별 y값이 중요하다는 것을 알게 됨. 이를 통해 각 landmark의 y값의 차이를 새로운 feature로 선정, 이를 통해 모델링을 수정한 결과 학습 데이터에 대해서는 기존보다 F1 score는 기존 모델링보다 낮았으나 학습 데이터 이외의 데이터에 대해서는 정확한 분류를 나타내었습니다. 이로 인해 기존의 x,y,z좌표에 대한 feature를 각 exercise별 y_height로 대체하였습니다. 또한 각 image별 landmark에 대한 x,y,z 좌표의 정형 데이터를 사용하였기 때문에 딥러닝보다는 머신러닝 모델을 고려하게 되었습니다.

### 2. 카운팅 모델
**[폴더] 3_autoEncoder**
<br>

- 운동 동작의 특정 각도에서 카운트를 증가시키는 방식으로 구현.
- 수집된 영상 데이터를 프레임 단위로 분할하고, 스켈레톤 좌표를 이용하여 필요한 부분(팔꿈치, 무릎 등)을 추출.
- 추출된 부분에서 키포인트(관절)의 좌표를 이용하여 팔꿈치나 무릎의 각도를 계산.
- 계산된 각도를 기반으로 카운트를 증가시킴.

<br>

<!-- 위 또는 아래 하나 선택
1. 주기 감지 및 시작:
- 주기가 있는 운동의 경우, 특정 운동 동작의 주기(예: 스쿼트에서 일어서는 주기)를 감지.
시작 지점을 정의하고 주기의 시작을 감지하는데 사용될 특정 각도를 설정.
2. 데이터 수집 및 전처리:
- 영상 데이터를 프레임 단위로 분활하고 필요한 부분(팔꿈치, 무릎 등)을 추출.
3. 각도 계산 및 주기 판별:
- 각 관절의 좌표를 이용하여 팔꿈치나 무릎의 각도를 계산.
설정한 시작 지점에서 주기가 시작되면, 각도의 변화를 감지하여 주기의 진행을 확인.
4. 주기 내 각도 기준 설정:
- 특정 주기 내에서 카운트를 증가시킬 때 사용될 각도 기준을 설정.
예를 들어, 스쿼트에서 일어서는 단계에서 무릎이 특정 각도 이상으로 굽혔을 때 카운트를 증가시키는 등의 조건을 설정.
5. 주기 내 각도 검사 및 카운트:
- 주기가 진행되는 동안, 각 프레임에서 설정한 각도 기준을 충족하는지 확인.
기준을 충족하면 카운트를 증가시킴. -->

<br>

### 3. 정확도 추론 모델
**[폴더] 4_imgModel**
- count 별 
<img width="1178" alt="accuracy model" src="https://user-images.githubusercontent.com/134662856/279439884-29b9dbc0-491f-4d8d-addb-76c68f7591d1.png">
- Data set의 condition을 활용하여 운동의 정확도를 기준으로 잡았습니다(conditions가 전부 True이면 정확한 자세, 그 외에는 부정확한 자세) 분류 모델과 마찬가지로 LSTM모델을 사용해보았으나 F1-score, Accuracy 둘다 성능이 낮았습니다. 이에 input-Data의 Features 또한 변형해 보았으나 성능에 유의미한 차이를 나타내지는 않았습니다. 이에 데이터를 재차 분석해 본 결과 두 가지 문제점을 찾을 수 있었습니다.
- Conditions의 main feature 우리가 정확도에 중점을 둔 것은 Data의 conditions였습니다. random forest를 활용하여 conditions를 결정하는 importances를 구해본 결과, 한 두개의 landmark만 영향을 끼친다는 것을 알 수 있었습니다. 이에 각 운동별 conditions에 영향을 주는 feature에 가중치를 부여해 보았습니다

- 한 sequence의 count차이 데이터에 활용되는 이미지를 분석해본 결과, 한 sequence로 묶은 이미지당 운동 count가 각자 다른 것을 알 수 있었습니다. 이를 해결하기 위해 데이터를 다음과 같은 단계로 전처리를 진행하였습니다. 2-1 같은 운동 동작에 대해 한 인물이 진행하는 것이기 때문에 sequence마다 주기성이 존재했습니다. 2-2 Data를 sequence로 묶은 후 , 1번에서 구한 Main features를 기준, 오름 차순(또는 내림차순)으로 정렬하였습니다. 그 후 최소(최대)값 기준 Sequence의 첫 번째, 그 다음 값을 32번째, 2번째, 31번째 ...등으로 반복하여 운동 동작 1회를 학습하게 하는 sequence processing과정을 거쳐 위와 같은 문제를 해결할 수 있었습니다
<br>

## 3. 서비스 구현
- ~~
<br>

# Badges 
<div align=center>
  <img src="https://img.shields.io/badge/python-3776AB?style=for-the-badge&logo=python&logoColor=white"> 
  <br>
  <img src="https://img.shields.io/badge/visualstudiocode-007ACC?style=for-the-badge&logo=visualstudiocode&logoColor=white"> 
  <img src="https://img.shields.io/badge/googlecolab-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=white"> 
  <img src="https://img.shields.io/badge/Anaconda-44A833?style=for-the-badge&logo=Anaconda&logoColor=white">  
  <img src="https://img.shields.io/badge/openCV-5C3EE8?style=for-the-badge&logo=openCV&logoColor=white">  
  <br>
  <img src="https://img.shields.io/badge/pandas-150458?style=for-the-badge&logo=pandas&logoColor=white"> 
  <img src="https://img.shields.io/badge/numpy-013243?style=for-the-badge&logo=numpy&logoColor=white"> 
  <img src="https://img.shields.io/badge/tensorflow-FF6F00?style=for-the-badge&logo=tensorflow&logoColor=white"> 
  <img src="https://img.shields.io/badge/scikit learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white"> 
  <img src="https://img.shields.io/badge/Keras-D00000?style=for-the-badge&logo=Keras&logoColor=white">  
  <br>
  <img src="https://img.shields.io/badge/Flask-000000?style=for-the-badge&logo=flask&logoColor=white"> 
  <img src="https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white"> 
  <img src="https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=Node.js&logoColor=white">
  <img src="https://img.shields.io/badge/javascript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black"> 
  <img src="https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black">  
  <br>
  <img src="https://img.shields.io/badge/ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=black"> 
  <img src="https://img.shields.io/badge/amazonaws-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white"> 
  <br>
  <img src="https://img.shields.io/badge/git-F05032?style=for-the-badge&logo=git&logoColor=white">
  <img src="https://img.shields.io/badge/github-181717?style=for-the-badge&logo=github&logoColor=white">
  <img src="https://img.shields.io/badge/notion-000000?style=for-the-badge&logo=notion&logoColor=white">
  <img src="https://img.shields.io/badge/slack-4A154B?style=for-the-badge&logo=slack&logoColor=white"> 
</div>
