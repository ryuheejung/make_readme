<img width="1100" height="200" alt="header" src="https://user-images.githubusercontent.com/134662856/278376964-5d4785b3-cf33-4d04-a447-b0a5cb1b8fc3.png">
<!--이미지 내에 타이틀 내용 기입할 것,(이미지만 보고 무슨 내용인지 직관적으로 알수있게끔) -->

<br>

# Project title
### 5가지 운동 종류의 정확한 자세를 추론하고 카운팅을 지원하는 운동 인식 인공지능 모델

<br>

# Description
### app 이 무슨 일을 하는지
- 운동 영상을 기반으로 운동의 종류를 판단하고, 해당 운동 동작에 대한 정확도를 카운트마다 평가
- 정확도 평가 모델에 따라 개수를 카운팅 해주는 인공지능 코칭 서비스를 제공함으로써 보다 객관적인 운동 판단이 가능해짐

### 왜 이 기술을 사용했는지
- 이미지, 비디오 기술 및 사람 동작 인지 연구가 지속적으로 성장하나 헬스산업 분야에서 
적용되어지고 있지 않음
- 2023년 9월 기준, ..( 우리 기술이 다른 기술보다 장점인 부분 어필하기)다른 app들은 한가지운동만 지원, 한 각도에서만 지원 등등을 설명하며 우리서비스는 여러각도에서 찍어도 가능 총 5가지의 운동을 지원한다는 점을 어필할 것,


### 미래에 발전시키고 싶은 내용
- 현재 checkmate는 운동 종류마다 정확한 자세를 판단하는 기준이 3-5가지인 것을 통합하여 모든 조건이 정확한 경우에만 정확한 자세라고 학습을 시켰음
- 조건에 해당하는 condition 을 하나하나 학습 시키면 정확도 향상을 기대할 수 있음.
- 현재의 기술 개발은 영상 업로드만 가능, cam 과 연결하여 실시간 평가가 가능하게 발전시킬 예정.


<br>

# Table of contents
[1) Project title](#Project-title) <br>
[2) Description](#Description) <br>
[3) Table of contents](#Table-of-contents) <br>
[4) Schedule](#Schedule) <br>
[5) Service demonstration video](#Service-demonstration-video) <br>
[6) Project process](#Project-process) <br>
[7) Badges](#Badges) <br>

<br>

# Schedule
wbs 참고해서 우리 일정표 추가할 것
<br>

<img width="1178" alt="프로젝트 일정표" src="https://user-images.githubusercontent.com/130980274/280462712-4487f38e-1e11-4263-95fb-578c9a3888ba.png">

<br>

# Service demonstration video
![image](https://user-images.githubusercontent.com/134662856/278372833-7c8a5ccf-e1ce-41ff-b877-a05f6087fcad.gif)

<br>

# Project process
## 1. 데이터 준비
**[폴더] 1_analysis**
1. 데이터 수집

- AI Hub에서 제공한 '피트니스 자세 이미지' 데이터셋을 수집.
약 1.04 TB 압축 파일로 정확도 추론에 이용되는 conditions, 운동 종류 등의 정보를 담고 있는 json file 과 하나의 영상을 총 32개의 이미지 jpg file 로 나누어 저장되어 있음.
이미지 파일은 mediapipe의 pose 를 이용하여 33개의 포인트 x,y,z 좌표를 추출한 후, labeling 파일과 merge하여 모델 학습에 이용되는 데이터셋을 구축.
 

<img width="1178" alt="dataset" src="https://github.com/ryuheejung/make_readme/assets/134662856/2d5c7857-6fa9-4212-8292-fbcf55764501">
<br>

2. 데이터 전처리 및 EDA
- 결측치 처리 : mediapipe에서 좌표값이 추출되지 않는 이미지 등의 이유로 결측치가 존재하는 경우, 해당 32개의 이미지를 모두 제거 (이유작성 필요, 충분한 데이터 수를 확보했기 때문에~~~ )

<img width="1178" alt="데이터셋구조" src="https://github.com/pulpo125/AIPlayer_Pling/assets/118874524/486a0b11-13da-4f86-897f-85cfafc1102b">


- 분류모델의 성능 평가에서 f1 score 가 상대적으로 낮은 두 운동의 EDA 를 진행.
- mediapipe 를 이용하여 좌표를 뽑아낸 경우 좌표값들이 정규화 되어 나오기 때문에 어느 각도, 이미지의 사이즈 등에서 자유도가 높은 편이다. 그렇지만 


## 2. EDA 및 모델링 
<img width="1178" alt="데이터전처리과정" src="https://github.com/pulpo125/AIPlayer_Pling/assets/118874524/5b575dc7-8efd-421c-b544-6c5fbff14426">

<br>



<img width="1178" alt="data flow of service" src="https://user-images.githubusercontent.com/134662856/279037487-81c602b8-144f-4e6b-a02a-76c3452a6e4b.png">
<!--전체적인 데이터(영상)이 들어오면 어떻게 처리되어 user 에게 보여지는지 한장의 이미지로 설명하기-->

### 1. 분류 모델
- EDA를 통해 알게된 각 exercise별 분류에 각 주요 부위 별 y값이 중요하다는 것을 알게 됨.
 이를 통해 각 landmark의 y값의 차이를 새로운 feature로 선정, 이를 통해 모델링을 수정한 결과 학습 데이터에 대해서는 기존보다 F1 score는 기존 모델링보다 낮았으나 학습 데이터 이외의 데이터에 대해서는 정확한 분류를 나타내었습니다. 이로 인해 기존의 x,y,z좌표에 대한 feature를 각 exercise별 y_height로 대체하였습니다. 또한 각 image별 landmark에 대한 x,y,z 좌표의 정형 데이터를 사용하였기 때문에 딥러닝보다는 머신러닝 모델을 고려하게 되었습니다.

### 2. 카운팅 모델
**[폴더] 3_autoEncoder**
<br>

<img width="1178" alt="추천과정" src="https://github.com/pulpo125/AIPlayer_Pling/assets/118874524/9c604725-0966-4160-b087-5bf1a68826b7">

- 일반적으로 운동 인식에서 이용되는 방법인  

### 3. 정확도 추론 모델
**[폴더] 4_imgModel**
- Data set의 condition을 활용하여 운동의 정확도를 기준으로 잡았습니다(conditions가 전부 True이면 정확한 자세, 그 외에는 부정확한 자세) 
분류 모델과 마찬가지로 LSTM모델을 사용해보았으나 F1-score, Accuracy 둘다 성능이 낮았습니다. 이에 input-Data의 Features 또한 변형해 보았으나 성능에 유의미한 차이를 나타내지는 않았습니다. 이에 데이터를 재차 분석해 본 결과 두 가지 문제점을 찾을 수 있었습니다.

1. Conditions의 main feature 
우리가 정확도에 중점을 둔 것은 Data의 conditions였습니다. random forest를 활용하여 conditions를 결정하는 importances를 구해본 결과, 한 두개의 landmark만 영향을 끼친다는 것을 알 수 있었습니다. 이에 각 운동별 conditions에 영향을 주는 feature에 가중치를 부여해 보았습니다


2. 한 sequence의 count차이
데이터에 활용되는 이미지를 분석해본 결과, 한 sequence로 묶은 이미지당 운동 count가 각자 다른 것을 알 수 있었습니다. 이를 해결하기 위해 데이터를 다음과 같은 단계로 전처리를 진행하였습니다.
2-1 같은 운동 동작에 대해 한 인물이 진행하는 것이기 때문에 sequence마다 주기성이 존재했습니다.
2-2 Data를 sequence로 묶은 후 , 1번에서 구한 Main features를 기준, 오름 차순(또는 내림차순)으로 정렬하였습니다. 그 후 최소(최대)값 기준 Sequence의 첫 번째, 그 다음 값을 32번째, 2번째, 31번째 ...등으로 반복하여 운동 동작 1회를 학습하게 하는 sequence processing과정을 거쳐 위와 같은 문제를 해결할 수 있었습니다 
<img width="1182" alt="이미지" src="https://github.com/pulpo125/AIPlayer_Pling/assets/118874524/5617f64d-ecb3-48e4-a143-df331752b8a1">

## 3. 서비스 구현
### 1. DB 구축
**[폴더] 5_db**
![ERD](https://github.com/pulpo125/AIPlayer_Pling/assets/118874524/a1447de6-f855-4d54-bbcc-bdbf4669788b)

- 데이터 플로우에 따른 효율적인 DB를 구성하기 위한 ERD 설계를 진행했습니다. 플레이리스트 데이터셋의 경우 곡id 리스트로 구성되어있기 때문에 이 곡에 대한 정보를 song_meta 테이블에서 검색해야 합니다. 그러므로, 이 둘의 관계를 나타내는 song_in_ply 테이블을 만들어 ply_meta와 song_meta를 연결하였습니다. 이러한 구조로 MySQL를 사용하여 DB를 구축했습니다.
- 새로운 플레이리스트 생성이 완료되고 사용자가 플레이리스트를 만족하여 좋아요 버튼을 누르게 되면 이 플레이리스트 데이터는 해당 DB로 저장되어 오토인코더 재학습에 사용하게 됩니다. 이를 통해, 더 고도화된 추천 알고리즘을 기대할 수 있습니다.

### 2. 장고 웹 프로젝트
**[폴더] 6_web**

<img width="1154" alt="시스템설계도" src="https://github.com/pulpo125/AIPlayer_Pling/assets/118874524/16f47901-75a7-40a8-a095-d07155d77f8b">

- 모든 모델 학습과 함수 파일은 파이썬으로 개발되었습니다. 때문에, 파이썬 웹프레임워크인 장고를 사용하여 웹을 구축했습니다. 이는 AI Server와 Web Server를 따로 구축할 필요가 없어 비용적으로 효율이 있습니다.   

<br>

- 서비스 배포 과정
<img width="1181" alt="배포과정" src="https://github.com/pulpo125/AIPlayer_Pling/assets/118874524/1ec7c4a2-c006-48b6-9510-815674bb79e2">


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
