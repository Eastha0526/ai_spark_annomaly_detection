# AI SPARK 기계 이상탐지 대회
- - -
- https://aifactory.space/competition/
- 기계 설비 이상탐지 대회
- 공기압축기 데이터를 통하여 이상치를 탐지
- 대회 기간 : 23.04.03-23.04.21
 
> ■ 범천(주)은 ESG 가치를 담아 산업용 공기압축기를 개발하는 대덕연구개발특구 소재 기업입니다.

    제4회 연구개발특구 AI SPARK 챌린지는 산업기기 피로도를 예측하는 문제입니다.
    산업용 공기압축기 및 회전기기에서 모터 및 심부 온도, 진동, 노이즈 등은 기기 피로도에 영향을 주는 요소이며, 피로도 증가는 장비가 고장에 이르는 원인이 됩니다.
    피로도 증가 시 데이터 학습을 통해 산업기기 이상 전조증상을 예측하여 기기 고장을 예방하고 그로 인한 사고를 예방하는 모델을 개발하는 것이 이번 대회의 목표입니다.
    
## 모델 조건
- - -
- 모델링은 비지도 학습 방식으로 진행
- 평가 방법
  - macro F1-Score, Scikit-learn에 내장된 F1-score를 'macro'옵션으로 설정하여 평가에 사용
  
### 관련자료
![image](https://user-images.githubusercontent.com/110336043/234737743-3a6bfac3-062b-46df-9bb9-4c0f34dd6b53.png)
![image](https://user-images.githubusercontent.com/110336043/234737827-49653a7e-e8e2-4cf7-824a-171102e033d4.png)

- - -
### 사용 모델
- 가장 대표적인 이상탐지 모델 사용
  - Local Outlier Factor(LOF)
  - IsolationForest
  - AutoEncoder
  
- 위의 모델 중 베이스라인의 성능이 AutoEncoder가 가장 높게 나타났고 AutoEncoder를 사용
- AutoEnocoder의 층을 Dense층에서 LSTM층, GRU층으로 변경해보았을 때 성능이 더 높게 나타남
  - 대회측에서 시계열성이 있음을 공지
  - train 데이터의 경우 약 2000개의 소규모 데이터라 GRU의 성능이 더 좋았을 것이라고 생각
- 활성화 함수의 경우 relu함수 보다는 tanh함수를 사용했을 때 성능이 높게 나타났음
- 스케일링을 적용하였는데 대표적인 스케일링 방법인 StandardScaler, MinMaxScaler, MaxabsScaler, RobustScaler를 사용하였고 RobustScaler의 경우 가장 성능이 높게 나타났음
- 또한 PCA를 통한 그래프를 그림으로 실제 그래프를 통하여 이상치라고 예측을 한 후 Reconstruction error값을 조정하여 이상치라고 판단하였음
![image](https://user-images.githubusercontent.com/110336043/234739856-fea06d19-e29d-4f62-9265-9dbe94dc3ecc.png)

- 기록지
![image](https://user-images.githubusercontent.com/110336043/234740185-3d153406-2b78-4a26-a4b8-d841a9b5dd41.png)
- 위의 기록지처럼 기록을 하면서 모델의 성능을 늘리는 방식으로 튜닝을 하였음
- - -
- 결과
- F1-score : 약 0.95
- Reconstruction error값에 따라서 이상치를 정하기 때문에 그 오차를 설정하는 값에 따라 모델의 성능이 차이가 났기때문에 그 오차값을 정하는 것이 가장 어렵고 중요한 부분이라고 판단
- 데이터에서 주어진 Type별로 특성들이 달랐고, 그러한 도메인 지식의 부족으로 인하여 도메인에 대하여 조사가 필요하였고, 그러한 도메인적 지식이 더 존재하였다면 더 좋은 모델을 만들 수 있을 것이라고 판단.
![image](https://user-images.githubusercontent.com/110336043/234740843-299f0698-9909-4add-b03a-fa3f881bc712.png)
- EDA 중 발견한 사실 중 오토인코더의 경우 학습데이터는 모두 정상 데이터만 사용하여야 하는데 그래프를 통하여 나타난 저 수치는 이상치인지 이상치가 아닌지 제대로 확인할 수 없었다.
- 위와 같은 사실처럼 공기 압축기에 대한 도메인 지식이 존재하였다면, 더 좋은 결과가 나왔을 것이라고 생각하였고, 특성 공학을 하는 부분에서도 더 좋게 하여 더 좋은 분석이 됬을 것이다.
