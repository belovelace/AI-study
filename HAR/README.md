# HAR (Human Activity Recognition Using Smartphones) 데이터셋 정리

## 1. 데이터셋 개요

### 기본 정보
- **데이터셋명**: Human Activity Recognition Using Smartphones
- **제공처**: UCI Machine Learning Repository
- **DOI**: https://doi.org/10.24432/C54S4K
- **라이선스**: Creative Commons Attribution 4.0 International (CC BY 4.0)
- **기부일**: 2012년 12월 9일

### 데이터셋 특성
- **데이터 타입**: 다변량, 시계열
- **응용 분야**: 컴퓨터 과학
- **관련 태스크**: 분류, 클러스터링
- **인스턴스 수**: 10,299개
- **피처 수**: 561개
- **결측값**: 없음

## 2. 실험 설계

### 참가자 정보
- **인원**: 30명의 자원자
- **연령대**: 19-48세
- **장비**: 허리에 착용한 삼성 갤럭시 S II 스마트폰

### 활동 종류 (6가지)
1. **WALKING** - 걷기
2. **WALKING_UPSTAIRS** - 계단 올라가기
3. **WALKING_DOWNSTAIRS** - 계단 내려가기
4. **SITTING** - 앉기
5. **STANDING** - 서기
6. **LAYING** - 눕기

### 데이터 수집 과정
- **센서**: 가속도계(accelerometer)와 자이로스코프(gyroscope)
- **측정 축**: 3축(X, Y, Z) 선형 가속도 및 각속도
- **샘플링 주파수**: 50Hz
- **윈도우 크기**: 2.56초 (128 readings/window)
- **윈도우 오버랩**: 50%
- **라벨링**: 비디오 녹화를 통한 수동 라벨링

## 3. 데이터 전처리

### 신호 처리
- **노이즈 필터링**: 센서 신호에 노이즈 필터 적용
- **중력 분리**: Butterworth 저역 통과 필터를 사용하여 신체 가속도와 중력 가속도 분리
  - 중력 성분은 저주파 성분만 가진다고 가정
  - 0.3Hz 컷오프 주파수 사용

### 피처 추출
- **시간 도메인 변수**: 각 윈도우에서 시간 영역 통계값 계산
- **주파수 도메인 변수**: FFT를 통한 주파수 영역 특성 추출
- **총 561개 피처**: 시간 및 주파수 도메인 변수로 구성된 특성 벡터

## 4. 데이터 분할

### 훈련/테스트 분할
- **훈련 데이터**: 전체 참가자의 70%
- **테스트 데이터**: 전체 참가자의 30%
- **분할 방식**: 참가자 기준 랜덤 분할 (개인별 데이터가 훈련/테스트에 섞이지 않음)

## 5. 데이터 구조

### 제공 파일
- **X_train.txt / X_test.txt**: 561개 피처로 구성된 훈련/테스트 데이터
- **y_train.txt / y_test.txt**: 활동 라벨 (1-6)
- **features.txt**: 561개 피처명 목록
- **activity_labels.txt**: 활동 라벨과 활동명 매핑
- **subject_train.txt / subject_test.txt**: 참가자 ID

### 데이터 특성
- **레이블 분포**: 6개 클래스가 비교적 균등하게 분포
- **피처명 중복**: 일부 피처명이 중복되어 전처리 필요

## 6. 주요 전처리 이슈 및 해결방안

### 문제점
features.txt 파일에서 중복된 피처명이 존재하여 DataFrame 생성 시 오류 발생

### 해결방법
1. **중복 피처명 처리**: 중복된 피처명에 `_1`, `_2` 등의 접미사 추가
2. **전처리 함수 구현**:
   - `get_new_feature_name_df()`: 중복 피처명 수정
   - `get_human_dataset()`: 데이터 로딩 및 DataFrame 변환

## 7. 머신러닝 적용 예시

### 결정트리 적용 결과
- **기본 성능**: 약 86-87% 정확도
- **하이퍼파라미터 최적화**: GridSearchCV를 통한 성능 향상
  - `max_depth`: 8이 최적
  - `min_samples_split`: 추가 최적화로 미미한 성능 향상

### 주요 피처 중요도
시각화를 통해 어떤 피처가 레이블 값에 가장 큰 영향을 주는지 확인 가능

## 8. 데이터셋 활용 방법

### Python 코드 예시
```python
# UCI ML Repository에서 직접 로드
pip install ucimlrepo

from ucimlrepo import fetch_ucirepo

# 데이터셋 가져오기
human_activity_recognition = fetch_ucirepo(id=240)

# 데이터 (pandas DataFrame)
X = human_activity_recognition.data.features
y = human_activity_recognition.data.targets

# 메타데이터 확인
print(human_activity_recognition.metadata)
print(human_activity_recognition.variables)
```

## 9. 관련 연구 및 확장

### 업데이트된 버전
- **확장 데이터셋**: http://archive.ics.uci.edu/ml/datasets/Smartphone-Based+Recognition+of+Human+Activities+and+Postural+Transitions
- **추가 기능**: 
  - 자세 전환 라벨 포함
  - 전처리되지 않은 원시 관성 신호 제공

### 인용 논문
- **원논문**: "A Public Domain Dataset for Human Activity Recognition using Smartphones" (Anguita et al., 2013)
- **학회**: European Symposium on Artificial Neural Networks

## 10. 실무 적용 시 고려사항

### 데이터 전처리의 중요성
실제 데이터 분석에서는 학습/예측보다 데이터를 알맞은 형태로 가공하는 전처리 과정의 비중이 훨씬 크며, 이 부분에서 성능 차이가 발생

### 하이퍼파라미터 최적화 효율성
너무 많은 하이퍼파라미터 최적화는 시간 대비 성능 향상이 미미하므로 1-2개 주요 파라미터에 집중하는 것이 효율적

## 11. 활용 분야

- **헬스케어**: 일상생활 활동 모니터링
- **피트니스**: 운동 패턴 분석
- **스마트 디바이스**: 컨텍스트 인식 서비스
- **노인 케어**: 낙상 감지 및 활동 패턴 분석
- **연구**: 인간 행동 인식 알고리즘 개발 및 평가

이 데이터셋은 스마트폰 센서를 활용한 인간 활동 인식 연구의 표준 벤치마크 데이터로 널리 사용되고 있다.
