<header id="header">
      <h2>Project Info.</h2>
<br>     
          
- 제목 : 정형 및 비정형데이터를 활용한 금리예측모델
  
- 목표 : 비정형 데이터의 잠재적 예측 정보(지정학적 리스크 등)를 자연어 처리 기법을 통해 정량화하여 반영한 금리 예측 모델을 개발하고, 금리 변화의 원인을 이해하고 미래 변동을 합리적으로 예측할 수 있도록 하는 인사이트 제공
  
- 기간 : 2025년 02월 13일 ~ 2025년 04월 10일

- 참여 구분 : 팀 프로젝트 (팀원)

- 담당 : 기획서 작성, 비정형 데이터 수집 및 전처리, EDA, 뉴스 제목 감성분석, 딥러닝 모델 학습 및 생성

- 주최 기관/대회명 : 멀티캠퍼스/데이터분석가 부트캠프 19회차 최종프로젝트
<br>
<main>
    <section>
      <h2>Stacks</h2>
<h3>Language</h3>
<img src="https://img.shields.io/badge/python-3776AB?style=for-the-badge&logo=python&logoColor=white">

<h3>Environment</h3>
<img src="https://img.shields.io/badge/jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white">
<img src="https://img.shields.io/badge/googlecolab-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=white">
<img src="https://img.shields.io/badge/anaconda-44A833?style=for-the-badge&logo=anaconda&logoColor=white">
<img src="https://img.shields.io/badge/AWS-000000?style=for-the-badge&logo=amazonaws&logoColor=white">
<h3>Database</h3>
<img src="https://img.shields.io/badge/googledrive-4285F4?style=for-the-badge&logo=googledrive&logoColor=white">
<img src="https://img.shields.io/badge/mysql-4479A1?style=for-the-badge&logo=mysql&logoColor=white">
<h3>Modeling</h3>

- 감성 분류 모델 : Finbert, KF-Deberta
- Deep learning : Hybrid model (transformer + (LSTM, GRU))
<h3>Framework</h3>
<img src="https://img.shields.io/badge/tensorflow-FF6F00?style=for-the-badge&logo=tensorflow&logoColor=white">
<h3>Libraries</h3>
<img src="https://img.shields.io/badge/pandas-150458?style=for-the-badge&logo=pandas&logoColor=white">
<img src="https://img.shields.io/badge/numpy-013243?style=for-the-badge&logo=numpy&logoColor=white">
<img src="https://img.shields.io/badge/scikitlearn-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white">
<img src="https://img.shields.io/badge/seaborn-000000?style=for-the-badge&logo=seaborn&logoColor=white">
<img src="https://img.shields.io/badge/matplotlib-000000?style=for-the-badge&logo=matplotlib&logoColor=white">
<img src="https://img.shields.io/badge/beautifulsoup-000000?style=for-the-badge&logo=beautifulsoup&logoColor=white">

<h3>Communication</h3>
<img src="https://img.shields.io/badge/slack-4A154B?style=for-the-badge&logo=slack&logoColor=white">
<img src="https://img.shields.io/badge/zoom-0B5CFF?style=for-the-badge&logo=zoom&logoColor=white">
<img src="https://img.shields.io/badge/notion-000000?style=for-the-badge&logo=notion&logoColor=white">
<img src="https://img.shields.io/badge/Gather-6F2991?style=for-the-badge&logo=gather&logoColor=white">
    </section><br><br>

## 뉴스 제목 감성분석 프로세스
#### 1. 데이터 전처리
- Test Data : 구글 뉴스에서 '금리'으로 검색한 뉴스의 제목 (2008-2025.02)
- 결측치, 이상치, 특수문자, 중복값 제거 (결측치 존재 시 오류남)
- 토큰화 : kakaobank/kf-deberta-base
- 데이터 특징 : 영문, 한 문장, 특수문자 있음.
---------------------------------------------
#### 2. 초기 모델용 데이터 수집
- 구글 뉴스 제목을 자동 레이블링해 줄 감성 분류모델을 학습시키기 위한 Data 확보
  1) KcBERT Pre-Training Corpus (Korean News Comments)
    - 총 4,846 개
    - 긍정, 부정, 중립으로 레이블 되어 있음.
  2) T5-large-korean-news-title-klue-ynat
    - 총 7,466 개 (한국 연합뉴스 Dataset, 경제 뉴스만 추출하여 사용)
    - 주제 분류용 Dataset으로 감성 레이블링 되어있지 않음
---------------------------------------------
#### 3. 초기 모델(Finbert) 생성 및 학습용 데이터 레이블링
1) 초기 모델 생성
    - 적은 데이터로 과적합 발생하였으나, 데이터 증강 및 하이퍼 파라미터 튜닝으로 극복하였음.
    - 최종 결과 : Loss - 0.4369 / Accuracy - 0.8237
2) 학습용 데이터 레이블링 및 병합
    - 초기 분류모델로 ynat 데이터에 감성 예측 수행, label 값 저장
    - 모든 데이터 병합 -> 총 12,312개의 경제 뉴스 감성 분류 데이터 생성
---------------------------------------------
#### 4. 감성 분류 모델 학습 및 생성
1) 레이블링이 완료된 Training Data로 KF-Deberta 감성 분류 모델 학습 및 생성 
2) 최적의 학습률(Learning Rate) 검사 : 변동성 증가 직전 최소 손실 지점
    - 테스트 범위 : 1e-8 ~ 5e-6 / max_iter = 100
    - 결과 : 4.45e-6
3) CosineAnnealingWarmupRestarts 학습 적용
    - 모델의 초기 학습 불안정성을 보완하기 위해 점진적으로 학습률을 선형적으로 증가시키며 학습(Warmup)
    - 최적 학습률(best LR)에 도달하면 최소 학습률(best LR*0.01)까지 학습률을 줄여가며 학습(CosineAnnealing)
    - 한 사이클이 끝나면 다시 반복 실행(Restarts)
    - 한 사이클 크기: warmup steps(전체의 10%)를 제외한 전체 steps의 15%
4) 하이퍼 파라미터 튜닝
    - 모델링 테스트 총 23회 진행함
    - 최종 주요 파라미터: max len = 195, weight_decay = 1e-4, batch_size = 32, epoch = 4,
    - 최종 결과 : Loss - 0.3647 / Accuracy - 0.8607
---------------------------------------------
#### 5. 감성 분류 모델 평가
- sklearn의 cross_val_score() 함수로 5겹 교차검증 실시
- 감성 분석 정확도는 약 75%로 나타남.
