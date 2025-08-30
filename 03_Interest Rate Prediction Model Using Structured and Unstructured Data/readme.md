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

## Ⅰ. 금리 예측 모델 최종 결과
- 정형데이터만 학습시킨 XGBoost 모델을 최종 모델로 선정함.<br>
- RMSE : 0.0655 / MAE : 0.0406
#### 📊  모델 평가 결과
| 데이터 구성 | 지표 | LightGBM | XGBoost | RandomForest | LSTM+GRU |
|:---|:---|:---:|:---:|:---:|:---:|
| **정형데이터** | RMSE | 0.0805 | 0.0655 | 0.0796 | 0.6403 |
|(위와 동일)| MAE | 0.0563 | 0.0406 | 0.0557 | 0.5335 |
| **정형+비정형 데이터** | RMSE | 0.0831 | 0.0918 | 0.1787 | 1.1698 |
|(위와 동일)| MAE | 0.0634 | 0.0534 | 0.1319 | 1.0854 |

<br><br>
## Ⅱ. 담당했던 작업 상세 설명<br>
### 1. 뉴스 제목 감성분석 프로세스
#### 1-1. 데이터 전처리
- Test Data : 구글 뉴스에서 '금리'으로 검색한 뉴스의 제목 (2008-2025.02)
- 결측치, 이상치, 특수문자, 중복값 제거 (결측치 존재 시 오류남)
- 토큰화 : kakaobank/kf-deberta-base
- 데이터 특징 : 영문, 한 문장, 특수문자 있음.
---------------------------------------------
#### 1-2. 초기 모델용 데이터 수집
- 구글 뉴스 제목을 자동 레이블링해 줄 감성 분류모델을 학습시키기 위한 Data 확보
  1) KcBERT Pre-Training Corpus (Korean News Comments)
    - 총 4,846 개
    - 긍정, 부정, 중립으로 레이블 되어 있음.
  2) T5-large-korean-news-title-klue-ynat
    - 총 7,466 개 (한국 연합뉴스 Dataset, 경제 뉴스만 추출하여 사용)
    - 주제 분류용 Dataset으로 감성 레이블링 되어있지 않음
---------------------------------------------
#### 1-3. 초기 모델(Finbert) 생성 및 학습용 데이터 레이블링
1) 초기 모델 생성
    - 적은 데이터로 과적합 발생하였으나, 데이터 증강 및 하이퍼 파라미터 튜닝으로 극복하였음.
    - 최종 결과 : Loss - 0.4369 / Accuracy - 0.8237
2) 학습용 데이터 레이블링 및 병합
    - 초기 분류모델로 ynat 데이터에 감성 예측 수행, label 값 저장
    - 모든 데이터 병합 -> 총 12,312개의 경제 뉴스 감성 분류 데이터 생성
---------------------------------------------
#### 1-4. 감성 분류 모델 학습 및 생성
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
#### 1-5. 감성 분류 모델 평가
- sklearn의 cross_val_score() 함수로 5겹 교차검증 실시
- 감성 분석 정확도는 약 75%로 나타남.
---------------------------------------------
<br><br><br>
### 2. 딥러닝 금리예측모델 모델링
#### 2-1. 모델 설계
#### -  1번 모델: LSTM + GRU
- 시계열 데이터를 학습하고 장기 의존성을 증가시키기 위해 시퀀스 데이터를 생성함
- 3개월을 최적의 window size로 선정하여 데이터 전처리함
- 비정형데이터인 뉴스를 정형데이터로 변환하여 모델에 학습시킴(감성지수 산출)

#### 📊  모델링 테스트
| 순서 | 단계 | MAE | RMSE |
|---|---|---|---|
|1|raw data|1.7959|1.9668|
|2|Standard Scaler|0.7365|0.8105|
|3|Robust Scaler|0.7020|0.7862|
|4|MinMax Scaler|0.6371|0.6997| ← best model!
|5|로그 변환 + MinMax Scaler|1.0935|1.1991|
|6|지연 변수 + MinMax Scaler|2.1181|2.2477|
|7|PCA + MinMax Scaler|1.5128|1.5540|

#### -  2번 모델: transformer + (LSTM + GRU)
- 비정형데이터를 정형데이터와 함께 직접적으로 모델에 학습시켜서, 텍스트의 문맥 정보와 시계열의 시간 흐름을 함께 반영할 수 있도록 설계함

#### ⚙️ 모델 구조
- 비정형 데이터를 모델에 입력하기 위해 1번 모델에서 Transformer Encoder 층을 추가함.
 1) 비정형 데이터 처리 순서: Embedding 층(단어 벡터로 변환) → Transformer Encoder 층(문맥이 반영된 벡터로 변환) → GlobalAveragePooling1D 층(비대해진 정보량 줄임) → Dense 층(정형데이터와 결합) → 예측
 2) 정형 데이터 처리 순서 : LSTM 층 → GRU 층 → Dense 층(비정형데이터와 결합) → 예측
---------------------------------------------
#### 2-2. 하이퍼 파라미터 튜닝 후 결과

|| LSTM + GRU (1)| transformer + (LSTM, GRU) |
|---|---|---|
|데이터 구성|금융데이터(정형)|금융데이터(정형), 금융뉴스 제목(비정형)|
|RMSE| 0.6403 | 1.1698 |

---------------------------------------------
#### 2-3. 결론
- 한국은행 기준금리는 일반적으로 전달 대비 0.25 %p, 금융위기 시 0.50 %p ~ 1.00 %p의 변동을 보이므로, 이번 프로젝트에서 생성한 딥러닝 모델의 경우 모두 평균 절대 오차(MAE)가 0.5%p를 초과하므로 유용한 예측력을 보이지 않는 것으로 확인됨
- 중요 사항 : 뉴스감성지수를 제거했을 때 더 높은 성능을 보임.

