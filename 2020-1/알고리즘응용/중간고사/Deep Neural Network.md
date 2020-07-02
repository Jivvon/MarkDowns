_201502119 정지원_

본 포스팅은 충남대 정상근 교수님의 강의를 참고하였습니다. 개인 공부를 기록하는 목적의 포스팅이며, 문제가 될 시 삭제하겠습니다.

# Deep Neural Network(DNN)

> Representation Learning
>
> Deep Learning - Representation Learning View
>
> Deep Learning - Data Transformation View

딥 러닝(Deep Learning)은 컴퓨터가 인간의 두뇌와 비슷한 모양의 인공 신경망을 형성하는 기계학습이다. 머신러닝과 비교하여 "딥"러닝은 말그대로 여러 개의 은닉층이 시간이 지나면서 쌓여 깊어진다는 의미이고, 여러 비선형 관계들의 모델링을 통해 높은 수준의 추상화를 시도한다.

딥 러닝에는 ANN, RNN, DNN 등 여러 알고리즘이 있는데 그 중 DNN을 Representation Learning과 Data Transformation의 시각에서 살펴보고 딥러닝의 학습과정을 알아본다.



## Representation Learning View

###### 어떻게 DNN은 사물의 특징을 스스로 파악할 수 있을까?

DNN의 핵심인 **Latent(Hidden) Variable, h** 은 가상의 값이다. 숫자이지만, 현실세계에서 counting할 수 없는 숫자이다. ($h$ : hidden variable) $h$는 무엇이든 될 수 있는데, DNN을 어떻게 설계하느냐에 따라 원하는 곳으로 수렴시킬 수 있다. 많은 input 데이터를 관측 가능한 값과 묶어 비슷한 성질을 가지도록하며 output 또한 고려하여 $h$의 범위를 축소시킬 수 있다.

즉, **잘 설계된 구조**와 **수많은 데이터**를 통해 학습된 $h$는 **사물의 특징을 설명**할 수 있게 된다.

#### Classical 머신 러닝 vs 딥 러닝

기존 분류 머신 러닝은 사물에서 특징을 추출하여 알고리즘을 적용하였다면, 딥 러닝에서는 특징을 **연산**할 수 있도록 숫자로 나타내어 적용한다. 예를 들어, 빨간색 차를 머신 러닝에서 인식할 때에는 '빨간색'이라는 속성을 가진 값으로 알고리즘을 적용하지만, 딥 러닝에서는 빨간색이라는 속성을 가진 값을 나타내는 '숫자'(Tensor)로 알고리즘을 적용한다.



## Data Transformation View

모든 데이터가 숫자(Tensor) 형태로 나타나는 딥 러닝에서의 분류 알고리즘을 생각해보자. 입력으로 들어오는 수많은 데이터 타입은 숫자로 이루어진 벡터값으로 나타낼 수 있고, 딥러닝에 적용하는 과정에서 목표값에 더 잘 도달하기 위해 각 데이터에 가중치를 줄 수 있다. 이렇게 가중치를 나타낸 벡터를 Convolution Filter라고 하며 이것을 가장 잘 표현한 딥러닝이 **Convolution Neural Network(CNN)**이다.

<img src="https://kr.mathworks.com/solutions/deep-learning/convolutional-neural-network/_jcr_content/mainParsys/band_copy_copy_14735_1026954091/mainParsys/columns_1606542234_c/2/image.adapt.full.high.jpg/1589087639613.jpg" alt="Convolutional Neural Network with many layers" style="zoom:50%;" />

CNN의 수많은 은닉층에서는 차원축소가 일어나는데 이는 은닉층 하나에서 일어났던 차원축소가(Single Layer) 여러 개의 은닉층에서(Multi Layer) 반복적으로 일어나는 것이다. 즉, CNN은 DNN의 응용이라 볼 수 있다.



#### 활용

최근에는 DNN은 음성인식에서 활용되고 있고, keras를 이용하여 직접 구현해볼 수 있다.

아래는 Classification DNN을 구현한 것이다.

```python
from keras import layers, models

class DNN(models.Sequential):
	def __init__(self, Nin, Nh_l, Nout):
		super().__init__()
		# 1 hidden layer
		self.add(layers.Dense(Nh_l[0], activation='relu', input_shape=(Nin,), name='Hidden-1'))
		# 2 hidden layer
		self.add(layers.Dense(Nh_l[1], activation='relu', name='Hidden-2'))
		# softmax : sum of output을 1로 만들어준다
		self.add(layers.Dense(Nout, activation='softmax'))
		# loss function : categorical_crossentropy
		self.compile(loss='categorical_crossentropy',optimizer='adam',metrics=['accuracy'])
```