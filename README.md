# ESAA YB 1조 — Numerai 방학 프로젝트 (컨퍼런스 제출본)

ESAA YB 1조가 2026년 여름 방학 프로젝트로 진행한 [Numerai](https://numer.ai) 토너먼트 모델입니다.
이 레포는 **2026년 7월 31일 컨퍼런스 제출 시점** 기준의 결과물을 보관합니다.

## 파이프라인

| 단계 | 내용 |
|---|---|
| 데이터 | Numerai v5.2 (train / validation) |
| 전처리 | feature selection → StandardScaler → IncrementalPCA(500) |
| 시계열 feature | era 평균 PCA 값의 lag1 / lag2 (총 1,500개 feature) |
| 베이스 모델 | CatBoost, XGBoost, LightGBM, RandomForest, ExtraTrees, Ridge, ElasticNet |
| 앙상블 | 7개 모델 예측의 rank-average (MLP·TabNet은 성능 문제로 제외) |
| 후처리 | feature neutralization (proportion 0.3, PCA 500개 기준) |

## 구성

```
notebooks/
  01_preprocessing_pca500_lag.ipynb       # feature selection, PCA 500, era lag feature
  02_full_refit_and_save_models.ipynb     # 전체 데이터 재학습 + 모델 파일 저장
  03_ensemble_neutralization.ipynb        # rank-average 앙상블, 중립화 비율 실험
  04_validation_diagnostics_유민주.ipynb   # Numerai validation diagnostics
  05_validation_diagnostics_서민정.ipynb   # Numerai validation diagnostics
models/                                   # 02 노트북으로 저장한 학습 완료 모델 + scaler / PCA
requirements.txt
```

※ 노트북은 Google Colab에서 실행했으며, 출력에 있던 Numerai 모델 ID 등은 공개용으로 지웠습니다.
※ 모델 파일은 v5.2 데이터 기준이라 v6.0 이후 라이브 데이터에는 그대로 쓸 수 없습니다.

## 역할 분담

| 이름 | 담당 |
|---|---|
| 민주연 | 전반적인 파이프라인 설계 (실험 구조·순서 기획, 공통 실험 기준 정리), PCA 차원 실험(100/300), 후처리(feature neutralization) 최종 구현, CatBoost · RandomForest · ExtraTrees 베이스 모델, rank-average 앙상블 결정, 전체 데이터 재학습(CatBoost · XGBoost · LightGBM · RandomForest · ExtraTrees) 및 모델 파일 저장 |
| 김서윤 | Feature selection, PCA(100) 실험, XGBoost 베이스 모델, neutralization 강도 비교(최적 0.3) |
| 유민주 | 전처리(selection + PCA 500), LightGBM 베이스 모델, 앙상블 조합 비교, validation diagnostics |
| 서민정 | Feature neutralization 초기 실험, PCA(100) + lag 실험, TabNet · Ridge · ElasticNet 베이스 모델, Ridge · ElasticNet 전체 데이터 재학습, 앙상블 조합 비교, validation diagnostics |
| 김시현 | Lag feature engineering, PCA(500) + lag1 + lag2 조합 실험(최종 채택) |

## 이후 개발

컨퍼런스 이후의 라이브 운영(자동 제출, v6.0 전환, 추가 모델 실험 등)은 이 레포 범위에 포함되지 않습니다.
