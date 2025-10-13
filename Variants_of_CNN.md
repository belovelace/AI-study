# SENet (CVPR 2018)

https://deep-learning-study.tistory.com/539https://ffighting.net/deep-learning-paper-review/vision-model/senet/

https://arxiv.org/abs/1709.01507https://github.com/hujie-frank/SENet

---

### 배경

전제: 목적 달성을 위해 특정 정보를 더 ‘집중’ 하거나 ‘무시’ 해야한다.

한계: CNN에는 특정 정보만 ‘집중’ 하거나 ‘무시’ 하는(가중치를 주어 연산하는) 기능은 없다.

기대: 중요한 정보에 집중하는 기능 추가로 성능 개선을 기대

### 개념

SEet : cnn에서  중요한 부분에 집중할 수 있는 기능을 구현

### 방법

1. 채널별 중요도 적용
    
    중요도에 따라 가중치를 적용
    
2. 채널별 중요도 계산
    
    채널별 정보를 압축
    
    - 평균값을 사용하여 압축 (Global Average Pooling)
    - 위치 정보는 버리고 물체 정보만 남기게 된다
    
    파라미터 연산으로 구성
    
    - 압축만으로 ‘중요도’를 알 수 없음 → 학습 가능한 가중치 필요
    - 파라미터들이 중요도를 학습 (역전파)

역전파를 통해 파라미터들이 중요도를 학습하는 과정이 파라미터 연산이고 이는 압축만으로 채널의 중요도를 알 수 없기 때문

# GoogleNet (CVPR 2014)

(= Inception module)

https://arxiv.org/abs/1409.4842https://www.ytimes.co.kr/news/articleView.html?idxno=7149

https://arxiv.org/abs/1409.4842

(참고자료 추가) https://happy-support.tistory.com/64

---

### 특징

1. Inception module: 다양한 사이즈의 filter와 pooling 레이어로 이루어진 구조
2. 1x1 convolutional layer: 차원 감소를 위한 레이어
3. Auxiliary classifier: 효율적인 backpropagation을 위한 장치

-> 여러 크기의 필터를 병렬로 적용하여 다양한 스케일의 특징을 동시에 추출하는 모듈

## Inception의 장점

1.  다양한 스케일 정보 포착
    
    작은 물체부터 큰 물체까지 동시에 감지
    
2. 계산 효율성
    
    1×1 Conv로 파라미터 수 대폭 감소
    
3. 네트워크 깊이 증가 가능
    
    GoogLeNet은 22층으로 당시 매우 깊었음
    

### 1×1 Conv

: 1×1 크기의 필터를 사용하는 컨벌루션 연산

일반 )

3×3, 5×5 등으로 주변 픽셀을 봄 (공간 정보 활용)

1×1 Conv)

한 픽셀 위치에서 모든 채널을 섞음 (채널 정보만 활용)

→ 채널 수 조절과 계산량 감소에 사용

# Xception (CVPR 2017)

https://arxiv.org/abs/1610.02357v3

---

### 배경

전제: 일반 합성곱은 공간 정보(spatial)와 채널 정보(cross-channel)를 동시에 처리한다.

한계:

- 3D 공간(높이×너비×채널)에서 모든 상관관계를 한 번에 학습
- 계산 비용이 매우 높음 → 모바일/임베디드 환경에 부적합
- 파라미터 수가 많아 과적합(overfitting) 위험

### 기대

공간 정보와 채널 정보를 분리하여 처리하면 효율성을 높이면서도 성능을 유지/향상할 수 있다.

→ Depthwise Separable Convolution: 일반 Convolution을 두 단계로 분해하여 공간 상관관계와 채널 상관관계를 독립적으로 학습

### 방법

1. 공간 정보 처리 (Depthwise Convolution)

각 채널을 독립적으로 처리

- 채널간 상호작용은 무시
- 각 채널마다 공간적 특징만 추출

2. 채널 정보 처리 (Pointwise Convolution)

채널 간 상호작용 학습

- Depthwise에서 독립적으로 처리된 채널들을 결합
- 채널 간 관계를 학습하여 최종 출력 생성

@Depthwise(깊이별 합성곱) : 각 채널에 독립적으로 필터를 적용하는 연산 → 공간적 특징만 추출(파라미터 적어)

### 왜 분리하는가?

```html
일반 3×3 Convolution = 공간 학습 + 채널 학습 (동시)
                     ↓
Depthwise Separable = 공간 학습 (먼저) + 채널 학습 (나중)
```

**1**. 계산 효율성

- 동시 처리: K×K×C_in×C_out 연산
- 분리 처리: (K×K×C_in) + (C_in×C_out) 연산
- 연산량 약 1/8 ~ 1/10 감소

2. 파라미터 효율성

- 같은 파라미터 수로 더 깊은 네트워크 구축 가능
- 또는 같은 성능을 더 적은 파라미터로 달성

3. 학습 효율성

- 각 단계가 명확한 역할 분담 → 학습이 더 안정적
- 공간/채널 특징을 독립적으로 최적화 가능
