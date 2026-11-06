# adversarial-robust-nids

**적대적 공격에 강건한 AI 기반 네트워크 침입탐지 시스템 (Adversarially Robust AI-based NIDS)**

순천향대학교 정보보호학과 캡스톤디자인 PBL (No.5) 프로젝트입니다.
딥러닝 기반 네트워크 침입탐지 시스템(IDS)에 대한 적대적 예제 공격을 구현하고, 이에 강건한 탐지 모델을 설계·평가합니다.

> 현재 상태: 사전조사 및 설계 단계 (구현 전)

## 프로젝트 목표

- 딥러닝 IDS에 대한 적대적 공격(회피 공격)을 실험적으로 재현한다.
- 적대적 훈련 등 방어 기법을 적용해 강건한 탐지 모델을 만든다.
- 아래 세 가지 기준으로 평가한다.
  1. 기본 정확도: 95% 이상
  2. 공격 시 정확도 저하폭
  3. 방어 후 정확도 회복 (저하폭 10%p 이내)

## 실험 설계 (기본안)

| 항목 | 선택 | 확장 |
|---|---|---|
| 방어 대상 모델 | MLP | - |
| 데이터셋 | NSL-KDD | CICIDS2017 |
| 공격자 목표 | Evasion (회피) | - |
| 공격자 지식 | White-box | Black-box |
| 공격 범위 | Feature-space | - |
| 공격 기법 | FGSM, PGD | C&W |
| 방어 기법 | Adversarial Training | Feature Squeezing |

핵심 재현 대상 논문은 Debicha et al. (2021), *Adversarial Training for Deep Learning-based Intrusion Detection Systems* ([arXiv:2104.09852](https://arxiv.org/abs/2104.09852))이며, 여기에 C&W 공격, 다른 방어 기법과의 비교, 평가 기준별 비교표 등을 더하는 것이 이 프로젝트의 차별점입니다.

## 문서

| 문서 | 내용 |
|---|---|
| [project_design.md](project_design.md) | 설계 결정 사항, 선택 이유, 재현 대상 논문 분석 |
| [background_knowledge.md](background_knowledge.md) | IDS 기초, FGSM/PGD/C&W 등 배경지식 정리 |
| [references.md](references.md) | 참고문헌 목록 (링크) |

## 개발 환경 (예정)

- Python (딥러닝 프레임워크는 미정)
- [ART (Adversarial Robustness Toolbox)](https://github.com/Trusted-AI/adversarial-robustness-toolbox)

## 협업 규칙

### 브랜치
- `main`에는 직접 push하지 않습니다. 항상 브랜치를 새로 만들어 작업하고 Pull Request(PR)로 병합합니다.
- 브랜치 이름은 `종류/작업-내용` 형식의 영문 소문자와 하이픈으로 짓습니다.
  - 예: `feat/fgsm-attack`, `feat/adversarial-training`, `docs/update-references`, `fix/data-preprocessing`
  - 종류: `feat`(기능), `fix`(수정), `docs`(문서), `exp`(실험), `chore`(설정·잡무)
- 작업을 시작하기 전에 `main`을 최신으로 받습니다. (`git pull origin main`)
- 병합이 끝난 브랜치는 삭제합니다.

### 커밋과 PR 설명은 자세하게
- 커밋 메시지 첫 줄은 무엇을 했는지 한 줄로 요약하고, 본문에 **왜** 그렇게 했는지와 **무엇이 바뀌었는지**를 적습니다.
- PR에는 아래 내용을 반드시 적습니다.
  1. **변경 내용**: 무엇을 추가·수정했는지
  2. **변경 이유**: 왜 필요한지 (관련 논문이나 설계 결정이 있으면 링크)
  3. **실험 조건과 결과**: 데이터셋, 하이퍼파라미터, ε 값, 정확도 등 수치 (실험 PR인 경우)
  4. **확인 방법**: 어떻게 실행하고 검증했는지
  5. **리뷰어가 봐야 할 점**: 불확실한 부분, 알려진 한계
- 최소 한 명의 팀원이 리뷰한 뒤 병합합니다.

### 저장소에 올리지 않는 것
- 데이터셋, 모델 가중치, 논문 PDF (`.gitignore` 참고). 데이터셋은 각자 공식 출처에서 내려받아 `data/` 폴더에 둡니다.
- API 키, 토큰, 비밀번호 등 비밀 정보. 필요한 값은 `.env`에 두고 `.env.example`로 항목 이름만 공유합니다.
- 실수로 비밀 정보를 올렸다면 지우는 것만으로는 부족합니다. 즉시 해당 키를 폐기하고 팀에 알립니다.

## 팀

| 이름 | 역할 |
|---|---|
| 김윤중 | |
| 박수빈 | |
| 이혜원 | |
