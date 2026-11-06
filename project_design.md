# 프로젝트 설계 (PBL No.5 — 적대적 공격에 강건한 AI 기반 네트워크 침입탐지 시스템)

## 1. 결정해야 할 항목과 선택지

| # | 항목 | 선택지 | 추천 (기본안) |
|---|---|---|---|
| 1 | 방어 대상 모델 | MLP / CNN / LSTM / Autoencoder | **MLP** |
| 2 | 데이터셋 | NSL-KDD / CICIDS2017 / UNSW-NB15 | **NSL-KDD** (확장: CICIDS2017) |
| 3 | 공격자 목표 | Evasion(회피) / Poisoning(오염) | **Evasion** |
| 4 | 공격자 지식 | White-box / Black-box | **White-box** (확장: Black-box) |
| 5 | 공격 범위 | Feature-space / Problem-space | **Feature-space** (problem-space는 한계로 서술) |
| 6 | 공격 기법 | FGSM, PGD, C&W (최소 2개 필수) | **FGSM + PGD** (확장: C&W) |
| 7 | 방어 기법 | Adversarial Training / Feature Squeezing / Distillation / Ensemble | **Adversarial Training** (확장: Feature Squeezing) |

### 선택 이유 요약
- MLP: FGSM/PGD/C&W 모두 분류기 대상으로 설계된 공격이라 구현이 직관적이고 노트북에서 가볍게 돌아감. Autoencoder는 공격 정의 자체가 달라져 난이도가 높음.
- NSL-KDD: 41개 피처로 가볍고 입문용. 시간이 남으면 CICIDS2017로 확장.
- Evasion + White-box: 공격 트래픽을 "정상"으로 오분류시키는 것이 표준 시나리오. 3대 원조 논문(FGSM/PGD/C&W) 모두 white-box 가정.
- Feature-space: 구현 간단. 실제 패킷 레벨의 기능 보존(problem-space)은 보고서의 "한계 및 향후 과제"로 서술.
- Defensive Distillation은 C&W로 뚫린 사례가 있어 메인 방어로는 리스크.

## 2. 구체화 문장 (초안)

> NSL-KDD 데이터셋으로 학습한 MLP 기반 이상탐지 IDS를 대상으로, white-box 환경에서 공격 트래픽을 정상으로 오분류시키는 feature-space evasion 공격(FGSM/PGD, 선택적으로 C&W)을 수행하고, adversarial training 기반 방어를 적용하여 그 효과를 측정한다.

## 3. 핵심 재현 대상 논문

**Debicha et al. (2021), "Adversarial Training for Deep Learning-based Intrusion Detection Systems", ICONS 2021**
https://arxiv.org/abs/2104.09852 (본문 직접 확인함)

위 기본안과 거의 일치하는 논문. 이 논문의 설계를 뼈대로 재현한다.

### 논문 설정
- 데이터: NSL-KDD의 KDDTrain+ 125,973건을 80/20 분할. 이진 분류(정상/공격).
- 전처리: 범주형 피처(protocol_type, service, flag) 수치화 + 전 피처 표준화, 레이블 원핫.
- 모델: MLP, 은닉층 2개(각 512, ReLU), Dropout 0.2, Adam, categorical cross-entropy, TensorFlow.
- 공격: FGSM, BIM, PGD (untargeted, white-box, evasion), ART로 구현. ε를 키워가며 정확도 측정.
- 방어: PGD로 만든 적대 샘플을 학습 데이터에 혼합(adversarial training). 변수는 ε_defense(0.1/0.3/0.5/0.7)와 적대 샘플 비율(30/50/70/90%).

### 논문 결과
- 정상 정확도 99.61%.
- 공격 후: FGSM 14.13%, BIM/PGD 8.85% (ε≈0.3 부근에서 거의 바닥).
- 적대적 훈련 후 강건성 향상. ε_defense가 클수록 효과가 좋았고(0.7 최고), 적대 샘플 비율은 뚜렷한 관계 없음.
- 정상 데이터 정확도는 약간 하락 (강건성-정확도 트레이드오프).

### 논문에 없는 부분 = 우리 프로젝트의 차별점
- C&W 공격 추가
- Feature Squeezing 등 다른 방어와 비교
- PBL 평가 기준(공격 시 저하폭, 방어 후 "저하 10%p 이내 회복") 직접 측정 및 비교표
- feature-space 한계(기능 보존, 이산 피처)에 대한 고찰
- (선택) CICIDS2017 확장

## 4. 보조 후보 (미확인)
Applied Sciences 2022, "Adversarial Robust and Explainable Network Intrusion Detection Systems Based on Deep Learning" — https://doi.org/10.3390/app12136451
NSL-KDD/CICIDS2017, FGSM/PGD/C&W 방어를 다루는 것으로 보이나 사이트 403으로 본문 미확인. 직접 열람 후 확인 필요.
