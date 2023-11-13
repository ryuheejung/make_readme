<img width="1100" height="200" alt="header" src="https://user-images.githubusercontent.com/134662856/278376964-5d4785b3-cf33-4d04-a447-b0a5cb1b8fc3.png">
<!--이미지 내에 타이틀 내용 기입할 것,(이미지만 보고 무슨 내용인지 직관적으로 알수있게끔) -->

<br>

# Project title
### 5가지 운동 종류의 정확한 자세를 추론하고 카운팅을 지원하는 운동 인식 인공지능 모델

<br>

# Description
### 앱 기능
- 운동 영상을 기반으로 운동의 종류를 판단하고, 해당 운동 동작에 대한 정확도를 카운트마다 평가
- 정확도 평가 모델에 따라 개수를 카운팅 해주는 인공지능 코칭 서비스를 제공함으로써 보다 객관적인 운동 판단이 가능해짐

### 기술 선택 이유
- 이미지, 비디오 기술 및 사람 동작 인지 연구가 지속적으로 성장하나 헬스산업 분야에서 
적용되어지고 있지 않음
- 2023년 9월 기준, ..( 우리 기술이 다른 기술보다 장점인 부분 어필하기)다른 app들은 한가지운동만 지원, 한 각도에서만 지원 등등을 설명하며 우리서비스는 여러각도에서 찍어도 가능 총 5가지의 운동을 지원한다는 점을 어필할 것,


### 미래 발전 방향
- 현재 checkmate는 운동 종류마다 정확한 자세를 판단하는 기준이 3-5가지인 것을 통합하여 모든 조건이 정확한 경우에만 정확한 자세라고 학습을 시켰음
- 조건에 해당하는 condition 을 하나하나 학습 시키면 정확도 향상을 기대할 수 있음.
- 현재의 기술 개발은 영상 업로드만 가능, cam 과 연결하여 실시간 평가가 가능하게 발전시킬 예정.


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

<img width="1100" alt="timeline" src="https://user-images.githubusercontent.com/134662856/282296372-79a4948a-6998-4d4c-86b9-3e186783c610.png">

<br>

# Service demonstration video
![image](https://user-images.githubusercontent.com/134662856/278372833-7c8a5ccf-e1ce-41ff-b877-a05f6087fcad.gif)

<br>

# Project process
## 1. 데이터 준비
**[폴더] 1_analysis**
1. 데이터 수집

- AI Hub에서 제공한 '피트니스 자세 이미지' 데이터셋을 수집.
- 약 1.04 TB 압축 파일로 하나의 영상을 총 32개의 이미지형태로 저장되어 있는 원천 데이터와 정확도 추론에 이용되는 conditions, 운동 분류에 이용되는 exercise 등 해당 운동 영상을 레이블링한 정보가 담긴 레이블링 데이터로 구성.
- 이미지 파일은 mediapipe의 pose 를 이용하여 33개의 포인트 x,y,z 좌표를 추출한 후, labeling 파일과 merge하여 모델 학습에 이용되는 데이터셋을 구축. 

<img width="1100" alt="dataset" src="https://user-images.githubusercontent.com/134662856/282302828-58f7aa1b-1afe-43b4-ad4d-2760f7c24533.png">

<br>

2. 데이터 구조

<img width="1178" alt="dataframe" src="https://user-images.githubusercontent.com/134662856/279438002-1f501f33-f631-44a3-bc64-b89a91288293.png">

- 데이터는 ~~식으로 이루어짐. 구조설명 필요.

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
- 초기 sequence의 image가 운동 순서대로 담겨져 있기 떄문에 LSTM model 고려
- 기존의 x,y,z 좌표에 대해서 feature를 두었을 때 학습되지 않은 데이터에 대해서 Accuracy가 떨어짐
- 각 landmark별 추출한 좌표를 활용하여 angle을 구한 후, feature에 추가, 여전히 Accuracy가 떨어짐
- EDA를 통해 알게된 각 exercise별 분류에 각 주요 부위 별 y값이 중요하다는 것을 알게 됨. 이를 통해 각 landmark의 y값의 차이를 새로운 feature로 선정, F1 score는 기존 모델링보다 낮았으나 학습 데이터 이외의 데이터에 대해서는 개서된 모습을 나타냄.
- 이미지에 대한 모델링을 하는것이 아닌 이미지로부터 추출한 좌표값, 정형 데이터로 변환하는 것이기 때문에 machine learning 모델이 더 좋다고 판단
- machine learning 모델 중 하나인 categorical boosting을 진행한 결과, F1-score는 떨어졌으나, 새로운 데이터에 대해서 높은 Accuracy를 보임

### 2. 카운팅 모델
**[폴더]
<br>

- 운동 동작의 특정 각도에서 카운트를 증가시키는 방식으로 구현.
- 수집된 영상 데이터를 프레임 단위로 분할하고, 스켈레톤 좌표를 이용하여 필요한 부분(팔꿈치, 무릎 등)을 추출.
- 추출된 부분에서 키포인트(관절)의 좌표를 이용하여 팔꿈치나 무릎의 각도를 계산.
- 계산된 각도를 기반으로 카운트를 증가시킴.

### 3. 정확도 추론 모델
**[폴더] 4_imgModel**
<br> 
<img width="1178" alt="accuracy model" src="https://user-images.githubusercontent.com/134662856/279439884-29b9dbc0-491f-4d8d-addb-76c68f7591d1.png">
- Count별, Data set의 condition을 운동의 정확도를 분류하는 데에 기준을 잡음(conditions가 전부 True이면 정확한 자세, 그 외에는 부정확한 자세)
- LSTM모델을 사용해보았으나 F1-score, Accuracy 둘다 성능이 낮음 -> 이에 input-Data의 Features 또한 변형해 보았으나 같은 결과가 나옴
- 이에 데이터를 재차 분석해 본 결과 두 가지 문제점을 발견함.
1. Main features 
- random forest를 활용하여 conditions를 결정하는 importances를 구해본 결과, 한 두개의 landmark만 영향을 끼친다는 것을 알 수 있었음.
-  이에 각 운동별 conditions에 영향을 주는 feature에 가중치를 부여
2. Exercise Count
- 한 sequence의 count차이 데이터에 활용되는 이미지를 분석해본 결과, 한 sequence로 묶은 이미지당 운동 count가 각자 다른 것을 알 수 있었음. 
- 이를 해결하기 위해 데이터를 다음과 같은 단계로 전처리를 진행하였음. 
 
    (2-1) 같은 운동 동작에 대해 한 인물이 진행하는 것이기 때문에 sequence마다 주기성이 존재함
    <br>
    (2-2) Data를 sequence로 묶은 후 , 1번에서 구한 Main features를 기준, 오름 차순(또는 내림차순)으로 정렬. 그 후, 최소(최대)값 기준 Sequence의 첫 번째, 그 다음 값을 32번째, 2번째, 31번째 ...등으로 반복하여 운동 동작 1회를 학습하게 하는 sequence processing과정을 거쳐 위와 같은 문제를 해결
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
