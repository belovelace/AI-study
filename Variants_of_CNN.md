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

⇒ 여러 크기의 필터를 병렬로 적용하여 다양한 스케일의 특징을 동시에 추출하는 모듈

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
