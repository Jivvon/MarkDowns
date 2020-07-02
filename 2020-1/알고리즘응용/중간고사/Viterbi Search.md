_201502119 정지원_

본 포스팅은 충남대 정상근 교수님의 강의를 참고하였습니다. 개인 공부를 기록하는 목적의 포스팅이며, 문제가 될 시 삭제하겠습니다.

# Viterbi Search

비터비(Viterbi) 서치는 관측된 사건들의 순서 중 **가장 가능성 높은 은닉 상태들의 순서**를 찾는 알고리즘이다. 중복되는 계산이 있기 때문에 동적 계획법이 적용되었다. 일반적으로 CDMA, GSM 같은 셀룰러 통신, 음성인식, 키워드 검출, 형태소 분석 등 *은닉 마르코프 모델 (Hidden Markov Model, HMM)*로 설명하는 문제는 대부분 비터비 알고리즘을 적용하기 때문에 굉장히 중요하다. 



#### 은닉 마르코프 모델 (Hidden Markov Model, HMM)

변화가 있는 상태의 순서가 있을 때, 현재의 상태는 모든 과거에 영향을 받는다. 하지만 실제로 모든 과거를 고려하는 것은 무리가 있기 때문에 (강한 제약을 걸어) 직전 몇 번의 과거만 현재에 영향을 미친다고 가정하는데, 이것을 (HMM assumption)이라 한다. 은닉 마르코프 모델은 어떠한 현상의 변화를 확률 모델로 표현한 마르코프 모델에 은닉된 상태와 확인 가능한 observation을 추가한 것이다.



#### Viterbi Search 과정

비터비 서치는 크게 2 step으로 나타낼 수 있다.

1. initialization step : 데이터를 은닉 마르코프 모델로 셋팅
2. recursion step : (비터비 서치 적용) **time step마다 각각의 state를 모두 확인**하며 **최적의 경로**를 찾는다.

여기서 **최적의 경로**는 모델과 관측치 시퀀스가 주어졌을 때 가장 확률이 높은 은닉상태의 시퀀스를 찾아(디코딩,decoding이라 한다)가는 과정을 의미한다. 최적의 상태를 저장하여 마지막으로 가장 확률이 높은 state를 찾았으면 해당 state에서 **역추적(backtracking)**을 하여 최적의 경로를 찾을 수 있다.



<img src="https://i.imgur.com/WzPD3id.png" alt="source: imgur.com" style="zoom:80%;" />

> 동그라미 안의 숫자를 비용(cost)라 하면 첫번째 state로 가는 경우의 수는 
> 1. 2 + 7 = 9 (S1,1)
> 2. 4 + 8 = 12 (S2,1)
> 이렇게 두 가지이다. 이 때에는 최적의 경로는 비용이 낮은 1번이 된다.
> 다음 두번째 state(S1,2)로 가는 경우의 수는
> 1. 9 (1-1) + 9 = 18
> 2. 12 (1-2) + 2 + 9 = 23
> 이므로 이 때의 최적의 경로는 1번이 된다.

위의 과정을 반복하여 마지막 state까지 찾은 후 역추적하여 최적의 경로를 찾을 수 있다.



#### 활용

hmmlearn 패키지의 HMM 클래스들이 제공하는 알고리즘에서 viterbi search를 사용할 수 있다.

```python
model = hmm.GaussianHMM(n_components=2, n_iter=len(X)).fit(X)
# model을 확인해보면 아래와 같이 viterbi 알고리즘을 적용한 것을 알 수 있다.
GaussianHMM(algorithm='viterbi', covariance_type='diag', covars_prior=0.01,
      covars_weight=1, init_params='stmc', means_prior=0, means_weight=0,
      min_covar=0.001, n_components=2, n_iter=500, params='stmc',
      random_state=None, startprob_prior=1.0, tol=0.01, transmat_prior=1.0,
      verbose=False)
```

