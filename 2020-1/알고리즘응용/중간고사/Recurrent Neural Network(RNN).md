_201502119 정지원_

본 포스팅은 충남대 정상근 교수님의 강의를 참고하였습니다. 개인 공부를 기록하는 목적의 포스팅이며, 문제가 될 시 삭제하겠습니다.

# Recurrent Neural Network(RNN)

RNN은 히든 노드가 방향을 가진 엣지로 연결돼 순환구조를 이루는(directed cycle) 인공신경망의 한 종류이다. 예제 문제를 풀며 예측하고, **예측값과 정답을 비교하여 잘 맞추는 방향으로 학습**하는 Supervised Learning의 하나이다. Sequence data를 순서대로 처리하므로 음성, 문자, 자연어처리와 같은 데이터 처리에 적합한 모델로 알려져 있다.

시퀸스 길이에 관계없이 인풋과 아웃풋을 받아들일 수 있기 때문에 다양하고 유연하게 구조를 만들 수 있다. 

### RNN의 구조

<img src="https://t1.daumcdn.net/cfile/tistory/2578204757AC186F2A" alt="img" style="zoom:70%;" />

RNN은 NN이 반복되는 체인으로 구성되어 있다. 순차적으로 들어오는 데이터를 순서대로 처리하며 예측하게 되는데, 동일한 모양이 반복되어 나타나므로 왼쪽의 그림을 보면 A개 순환하는 것처럼 보인다. 이를 Recurrent라고 한다. 아래의 Xt는 시간에 따른 입력을 표현한 것이고, 위의 h는 상태, A는 RNN 모듈이다. 좀 더 자세히 살펴보면 다음과 같다.

<img src="https://mblogthumb-phinf.pstatic.net/MjAxODAxMThfMjM3/MDAxNTE2Mjc0MzUwNjEw.lLbS6oEbl99TsYt8QZjnZqNMR8VsbdkGbhOfC9Vc4iwg.Fr9keq2hMYz_dSZECHCaz-n6-OuwS5rDxObCq_7RHwkg.PNG.jaeyoon_95/image.png?type=w800" alt="img" style="zoom:80%;" />

위의 Ot는 시간에 따른 출력(Output)값이다. St는 이전 시간의 h(hidden state)와 현재 시간의 x(input)에 의해 계산되는 상태(State)이다. U,V,W는 가중치(weight)로 각 상태와 값을 계산할 때 곱해준다.

<img src="https://mblogthumb-phinf.pstatic.net/MjAxODAxMThfMTAw/MDAxNTE2Mjc0NTU2MzI3.DJwidcWF3pQS3aICAJ3wA5Vh3RL4YddkkmPZYqfdmbMg.Ql36nhUxtu6fx0hl4yBqsMQglvA280D-YRnjDEv3zdIg.PNG.jaeyoon_95/image.png?type=w800" alt="img" style="zoom:67%;" />

출력값의 식은 $O_t = softmax(VS_t)$ 인데, 여기서 **softmax**는 **각각의 값의 편차를 확대시켜 큰 값은 상대적으로 더 크게, 작은 값은 상대적으로 더 작게 만든 후 정규화 시키는 함수**이다. softmax 함수를 적용하면 상대성을 가지게 되므로 전체 출력값은 0.0 ~ 1.0의 값으로 나타나며, 입력값의 합은 1이 된다.

<img src="https://cdn-images-1.medium.com/max/600/1*DtwGZO8CcRhEa4hgObry0A.png" alt="A beginner's guide to NumPy with Sigmoid, ReLu and Softmax ..." style="zoom:50%;" />

히든 State는 $S_t = f(U*x_t + W*S_{t-1})$ 인데, 여기서 활성함수(activation function)는 **비선형 함수**로 보통 ReLU나 하이퍼볼릭탄젠트(tanh)를 사용한다. 

전체적인 아이디어를 보면 다음과 같다.

1. (순전파 foward propagation) : 입력값(X)를 받아 히든 State를 계산한 후 출력값(Y)를 찾는 과정을 순서대로 반복하여 찾아내고
2. (역전파 backpropagation) : 역전파를 수행하여 parameter값들을 갱신하여 답을 찾는다.



RNN이 잘 학습된다면 실생활에서 정말 많이 쓰일 수 있는데, 단순히 'hello'에서 'hell'까지 입력 후 마지막 알파벳을 'o'로 예측할 수도 있고, '이 숙소 짱입니다'를 'Positive'로 인식할 수도 있으며, '알람 꺼줘'를 'Alarm.Off'로 대화모델에 사용될 수도 있다.

RNN은 히든 노드가 순환 구조를 띄어 과거의 데이터가 현재에 반영이 된다. 즉, 문장을 입력으로 주더라도 **앞뒤 문맥을 파악할 수가 있다**는 것이다. 하지만, 이런 RNN도 관련 정보와 그 정보를 사용하는 거리가 많이 떨어져있을 경우, **역전파(backward)시 그래디언트(gradient, 기울기)가 줄어들어** 학습능력이 크게 저하되는 것으로 알려져 있다. 이를 해결한 RNN을 LSTM(LSTMs, Long Short Term Memory networks)라고 한다.



#### 활용

품사 태깅(Part of speech Tagging)을 구현할 때 관측 가능도(Observation Likelihood)를 RNN을 통해 구현할 수 있다.

- 직전의 태그와 현재 태그가 동시에 나올 확률(Transition probability)은 단순 카운팅하여 구한다.

- Tag 자체가 나타날 가능도를 구하는 건 RNN을 통해 구한다.
- Token unit은 Word로 제한하고 BIO-tagging을 하지 않고 태그명을 바로 모델링한다.















