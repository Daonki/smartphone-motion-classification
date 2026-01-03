# 📱 Smartphone Sensor-based Motion Classification
### Hierarchical Activity Recognition using Sensor Features

본 프로젝트는 스마트폰 센서 데이터로부터 추출된 다차원 feature를 활용하여  
**정적/동적 행동을 먼저 분리한 뒤, 세부 행동을 단계적으로 분류하는  
계층적(Hierarchical) 모션 분류 모델**을 설계하고 성능을 분석한 프로젝트입니다.

---

## 📌 Project Overview

### 🎯 목표
- 스마트폰 센서 데이터 기반 행동 인식
- 센서 특성에 맞는 **문제 구조 재정의**
- 단일 다중분류 모델과 **단계적 분류 모델의 성능·해석력 비교**

---

## 🧩 Dataset

- **출처**: Smartphone Sensor Dataset (가속도계, 자이로스코프 기반)
- **샘플 수**
  - Train: 5,881
  - Test: 1,471
- **Feature 수**: 561 (통계 기반 센서 feature)
- **Target (6 classes)**
  - LAYING
  - SITTING
  - STANDING
  - WALKING
  - WALKING_UPSTAIRS
  - WALKING_DOWNSTAIRS

> 모든 feature는 사전 정규화(-1 ~ 1) 되어 있으며, 결측치는 존재하지 않습니다.

---

## 🧠 Problem Definition

### 왜 계층적 분류인가?
- **SITTING ↔ STANDING** 은 센서 패턴이 매우 유사
- 반면 **정적 / 동적 행동**은 물리적으로 명확한 차이를 가짐

👉 따라서 문제를 다음과 같이 분리

1. **Stage 1**: 정적(0) vs 동적(1) 이진 분류  
2. **Stage 2**
   - 정적 → LAYING / SITTING / STANDING
   - 동적 → WALKING / UP / DOWN

---

## 🔍 Step 1. Exploratory Data Analysis (EDA)

- RandomForest 기반 **Feature Importance 분석**
- 주요 결과
  - 중력 및 각도 관련 feature가 높은 중요도
  - LAYING 행동은 다른 행동과 명확히 구분 가능
  - 정적/동적 분리는 센서 기준으로 매우 안정적

---

## 🌲 Step 2. Baseline Modeling (RandomForest)

- 목적
  - 변수 중요도 기반 탐색
  - 데이터 구조 검증
- 결과
  - Validation Accuracy ≈ 0.97
  - 교차검증 결과 안정적
  - 데이터 불균형 문제 없음

---

## 🤖 Step 3. Hierarchical Deep Learning Models

### Stage 1. Static vs Dynamic Classification
- Model: Dense Neural Network
- Output: Binary (Sigmoid)
- Result:
  - Accuracy: **~1.00**
- 역할:
  - 최종 분류 ❌
  - **게이트(Routing) 역할**

---

### Stage 2-1. Static Activity Classification
- Classes: LAYING / SITTING / STANDING
- Result:
  - Accuracy: ~0.93
- Observation:
  - SITTING ↔ STANDING 혼동 존재
  - 센서 데이터의 물리적 한계로 해석

---

### Stage 2-2. Dynamic Activity Classification
- Classes: WALKING / WALKING_UP / WALKING_DOWN
- Result:
  - Accuracy: ~0.97
- Observation:
  - 동적 행동은 센서 패턴 차이가 뚜렷

---

## 🔗 Pipeline & Integration

- 단계별 모델을 함수 기반 파이프라인으로 통합
- 전처리 → Stage1 → Stage2 → 최종 예측
- 실제 서비스 적용을 고려한 구조 설계

---

## 🧪 (Optional) Multi-task Learning Model

- 하나의 shared trunk + 두 개의 head
  - is_dynamic (binary)
  - activity (6-class)
- is_dynamic 출력을 activity 분류에 조건 정보로 활용
- 단계적 분류 구조를 단일 모델로 통합한 실험

---

## 📊 Evaluation Metrics

- Accuracy
- Precision / Recall / F1-score
- Confusion Matrix

---

## 🔑 Key Insights

- 센서 데이터에서는 **문제 정의가 모델 성능만큼 중요**
- 계층적 분류는
  - 혼동 감소
  - 해석 가능성 증가
  - 실제 적용에 유리
- 단순 정확도보다 **구조적 설계의 타당성**이 중요함을 확인

---

## ⚠️ Limitations

- SITTING / STANDING 구분 한계
- 시계열 모델(LSTM 등) 미적용
- 개인별 센서 특성 미반영

---

## 🚀 Future Work

- CNN / LSTM 기반 시계열 모델 비교
- 사용자별 적응 모델
- 경량화 모델을 통한 모바일 환경 적용

---

## ✍️ Summary
> 스마트폰 센서 데이터를 이용해 정적/동적 행동을 분리하고  
> 단계적으로 세부 행동을 분류함으로써 혼동을 줄이고 해석력을 높인  
> 계층적 모션 분류 프로젝트
