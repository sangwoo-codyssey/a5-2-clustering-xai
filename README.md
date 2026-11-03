# A5-2 - AI 판단 이유를 사람에게 설명하기 (clustering-xai)

> 🚧 **미착수** — 디렉터리·레포 준비만 완료

K-Means 군집화로 고객 페르소나("누구인가")를 정의하고, 분류 모델로 승인/거절을 예측한 뒤,
SHAP 으로 "왜 그런 판정인가"를 Global(전체)·Local(개별) 해석으로 설명한다. 군집별 대표 고객의
Local SHAP 해석을 페르소나와 연결해, 비기술자도 읽을 수 있는 비즈니스 리포트로 정리한다.

- 과제 원문: 로컬 `../a5-2-clustering-xai.md`
- AI 심화 학습 · 머신러닝 · 80시간 · 필수 · 개인
- Python 3.10 이상 · shap·scikit-learn 버전을 `requirements.txt` 에 명시
- **선행: A5-1** — 원문의 "Mission 23" 이 A5-1 이다. 같은 `finance_data.csv` 와 A5-1 의 분류 모델을 쓴다

## 설치 및 실행

```bash
uv venv --python 3.12 .venv && source .venv/bin/activate
uv pip install -r requirements.txt   # uv 가 없으면 python -m venv .venv 후 pip install -r requirements.txt
```

실행 순서는 과제가 정해 두었다: `analysis_clustering` → `analysis_shap` → README 정리 (과제 §7).

## 데이터 정책 (과제 §4-1)

- 허용: 원본 CSV 기준 스케일링·결측치 처리·이상치 제거
- 금지: 임의 컬럼 추가, 외부 데이터 병합, 데이터 증강
- 군집화에는 타겟(`credit_score`·`is_overdue`)을 빼고 입력 변수만 쓴다

## 라이브러리 제약 (과제 §7)

| 구분 | 라이브러리 |
|---|---|
| 필수 | Scikit-learn, SHAP |
| 허용 | NumPy, Pandas, Matplotlib, Seaborn |
| 금지 | XGBoost·LightGBM 등 외부 모델링 라이브러리 (Scikit-learn 내장 모델만) |

SHAP 은 `TreeExplainer` 를 우선 쓰고 `KernelExplainer` 는 쓰지 않는다. 1만 건이면 1,000~2,000건 샘플링을 권장한다.

## 제출물 체크리스트

- [ ] 군집화 — 스케일링, Elbow + Silhouette 로 K 선정(Silhouette 우선), PCA 2차원 산점도 + Explained Variance Ratio, 군집 통계·페르소나
- [ ] Global — SHAP Summary Plot (PNG)
- [ ] Local — 승인·거절 각 1건 Waterfall Plot(PNG, 권장) 또는 Force Plot(HTML)
- [ ] Dependence Plot — 변수 1개 이상(Summary 상위 1~2개 + 비즈니스 의미 큰 변수 1개) + 인사이트 2문장 이상 (PNG)
- [ ] README — 군집 정의, 해석 결과(주요 변수·개별 예측 사유), 군집별 마케팅/리스크 관리 전략, 그래프마다 비즈니스 해석

## 보너스

- [ ] Streamlit 대시보드 (`app.py`)
- [ ] DBSCAN 또는 계층적 군집 vs K-Means
