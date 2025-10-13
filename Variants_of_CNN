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
