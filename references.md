# 참고 논문 레퍼런스 (PBL No.5 — 적대적 공격에 강건한 AI 기반 네트워크 침입탐지 시스템)

읽는 순서대로 정리. 1단계는 필독, 2~4단계는 프로젝트 진행 단계에 맞춰 참고.

## 1단계 — 적대적 공격 원조 논문 (개념 정립, 필독)

- **Explaining and Harnessing Adversarial Examples** (Goodfellow, Shlens, Szegedy, 2015)
  https://arxiv.org/abs/1412.6572
  FGSM 원 논문. 신경망이 적대적 예제에 취약한 이유(선형성 가설)와 FGSM 공격 기법, adversarial training 방어 아이디어의 원형 제시.

- **Towards Deep Learning Models Resistant to Adversarial Attacks** (Madry et al., 2018)
  https://arxiv.org/abs/1706.06083
  PGD 원 논문이자 adversarial training의 이론적 표준. 강건성을 min-max robust optimization 문제로 정식화.

- **Towards Evaluating the Robustness of Neural Networks** (Carlini & Wagner, 2017)
  https://arxiv.org/abs/1608.04644
  C&W 공격 원 논문. Defensive Distillation 방어를 우회하며 최적화 기반의 정교한 공격(L0/L2/L∞)을 제시.

## 2단계 — IDS 도메인 서베이 (전체 그림 잡기)

- **Adversarial Challenges in Network Intrusion Detection Systems: Research Insights and Future Prospects** (2024)
  https://arxiv.org/pdf/2409.18736
  ML-NIDS 전체 taxonomy, 공격/방어 총정리, 네트워크 트래픽 특유의 피처 간 상호의존성 문제까지 포괄. 최우선 추천.

- **A Review of the Duality of Adversarial Learning in Network Intrusion: Attacks and Countermeasures** (2024)
  https://arxiv.org/pdf/2412.13880
  공격과 방어를 쌍(duality)으로 정리한 리뷰.

- **Understanding the Adversary: A Survey of Adversarial Machine Learning in Network Intrusion Detection**
  https://www.sciencedirect.com/science/article/pii/S1574013726001036
  최신 서베이. 접근 가능 시 참고.

## 3단계 — 실험 재현 참고 (공격 실습용)

- **Adversarial Attacks Against Supervised Machine Learning Based Network Intrusion Detection Systems** (PLOS One)
  https://journals.plos.org/plosone/article?id=10.1371%2Fjournal.pone.0275971
  (정정) 본문 확인 결과 FGSM/PGD가 아니라 DCGAN으로 적대 샘플을 만들어 CICIDS2017의 Decision Tree/Logistic Regression에 evasion·poisoning을 가한 실험이며, 방어 기법은 없음. 코드: https://github.com/Ebtehaj431-hub/Adversarial_AI. 우리 설계와는 거리가 있어 참고 우선순위 낮음.

- **Constrained Network Adversarial Attacks: Validity, Robustness, and Transferability** (2025)
  https://arxiv.org/pdf/2505.01328
  네트워크 트래픽의 이산값/물리적 제약 문제(feature-space vs problem-space 공격)를 정면으로 다룸. 보고서의 "한계 및 고찰" 섹션 작성 시 필수 참고.

## 4단계 — 방어 기법 적용 논문

- **Adversarial Training for Deep Learning-based Intrusion Detection Systems** (2021)
  https://arxiv.org/pdf/2104.09852
  IDS에 adversarial training을 직접 적용한 사례. 구현 참고용으로 가장 실용적.

- **Feature Squeezing: Detecting Adversarial Examples in Deep Neural Networks** (Xu et al., NDSS 2018)
  https://arxiv.org/pdf/1704.01155
  Feature Squeezing 원 논문. 원래 이미지 대상이지만 원리 이해에 필수.

- **An Enhanced Ensemble Defense Framework for Boosting Adversarial Robustness of Intrusion Detection Systems** (Scientific Reports, 2025)
  https://www.nature.com/articles/s41598-025-94023-z
  여러 방어 기법을 조합한 최신 사례. 프로젝트 후반부 확장 아이디어로 참고.

## 핵심 재현 대상 (우리 설계와 가장 일치)

- **Adversarial Training for Deep Learning-based Intrusion Detection Systems** (Debicha et al., ICONS 2021)
  https://arxiv.org/abs/2104.09852
  NSL-KDD + MLP(512x2) + white-box evasion(FGSM/BIM/PGD) + PGD adversarial training + ART. 이 논문을 뼈대로 재현하고 C&W, Feature Squeezing, 10%p 회복 기준 측정을 추가한다. 상세 요약은 project_design.md 참고.

- **(부 레퍼런스) Constrained Network Adversarial Attacks: Validity, Robustness, and Transferability** (Grini et al., 2025)
  https://arxiv.org/abs/2505.01328
  MLP surrogate로 만든 적대 예제 중 도메인 제약을 위반하는 무효 샘플이 최대 80.3%임을 보이고, MLP/CNN/LSTM 간 전이성을 분석. 초록만 확인(데이터셋·공격 종류·코드는 미확인). 본문 열람 필요.

- (미확인 보조 후보) **Adversarial Robust and Explainable Network Intrusion Detection Systems Based on Deep Learning** (Applied Sciences, 2022)
  https://doi.org/10.3390/app12136451
  사이트 403으로 본문 미확인.

## 읽기 전략

시간이 부족하면 아래 4편만 우선 읽어도 설계·실험·보고서 작성에 필요한 배경지식은 충분함:
1. Goodfellow (FGSM)
2. Madry (PGD)
3. Adversarial Challenges in NIDS (서베이, 2409.18736)
4. Debicha et al. (2021) — 핵심 재현 대상 (PLOS One 논문은 설계가 달라 제외)
