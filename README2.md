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
[4) Project timeline](#Project-timeline) <br>
[5) Service demonstration video](#Service-demonstration-video) <br>
[6) Project process](#Project-process) <br>
[7) Badges](#Badges) <br>

<br>

# Project timeline
23.09.04 ~ 23.10.19 (약 6 주간)
<br>

<img width="1178" alt="timeline" src="https://user-images.githubusercontent.com/134662856/279435285-7c3efb4e-d202-4443-80d6-0da9506d3173.png">

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
<img width="1178" alt="data flow of service" src="https://user-images.githubusercontent.com/134662856/279430604-939f5f49-b12a-4d8a-a19d-91328ffc6540.png
">

### 1. 분류 모델
**[폴더] 2_sBert**

- categorical boost model 

### 2. 카운팅 모델
**[폴더] 3_autoEncoder**
<br>

- 스켈레톤 좌표를 이용하여 운동 종류마다 팔꿈치, 무릎 등 기준이 되는 포인트의 각도를 가지고 

### 3. 정확도 추론 모델
**[폴더] 4_imgModel**
- count 별 
<img width="1178" alt="accuracy model" src="
https://user-images.githubusercontent.com/134662856/279439884-29b9dbc0-491f-4d8d-addb-76c68f7591d1.png">

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
