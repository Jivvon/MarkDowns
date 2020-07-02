_201502119 정지원_

본 포스팅은 충남대 정상근 교수님의 강의를 참고하였습니다. 개인 공부를 기록하는 목적의 포스팅이며, 문제가 될 시 삭제하겠습니다.

# Fourier Transform

##### Fourier Transform 중요성

푸리에 변환은 임의의 입력 신호를 다양한 주파수를 갖는 주기함수들의 합으로 분해하여 표현하는 것이다. 여기서 임의의 입력 신호는 파동의 형태이어야하며, 분해를 하면 sin, cos으로 나타낼 수 있다. 이 때 sin과 cos 주기함수들의 특성을 파악하기가 어려운데, 푸리에 변환은 이를 더 쉽게 만들어준다. 즉, 시간의 도메인에서 주파수 도메인으로 변환하여 어려운 문제를 쉬운 문제로 바꿔준다는 것이다. 이렇듯 **푸리에 변환은 데이터를 보는 시점을 바꿔준다**.

파동 형태라면 어떠한 입력도 가능한만큼 통신 분야에서 활발하게 이용되는데, 특히 음성인식에서 많이 쓰인다. 소리의 경우 공기를 압축하는 정도에 따라 전달되는데, 시간의 도메인과 주파수 도메을 변환하면 해당 주파수만을 추출하여 제거할 수 있다. 실제로 노이즈 캔슬링에는 이산 푸리에 변환을 간결하고 빠르게 하는 **고속 푸리에 변환(Fast Fourier transform)**이 사용되고 있다.

하지만 단순히 x domain을 time에서 freqency로 바꾼다면 선행되는 주파수를 알 수 없다. 이 두 가지를 동시에 알게끔 하는 방법이 **Spectrogram**이다. Spectogram은 window를 정하여 계속 DFT를 돌려주며 나타내어 time과 frequency에 대한 정보를 동시에 보여준다.

고속 푸리에 변환은 scipy 패키지의 fftpack 서브패키지에서 제공하는 fft 명령으로 다음과 같이 사용할 수 있다.

```python
>>> from scipy.fft import fft
>>> # Number of sample points
>>> N = 600
>>> # sample spacing
>>> T = 1.0 / 800.0
>>> x = np.linspace(0.0, N*T, N)
>>> y = np.sin(50.0 * 2.0*np.pi*x) + 0.5*np.sin(80.0 * 2.0*np.pi*x)
>>> yf = fft(y)
>>> xf = np.linspace(0.0, 1.0/(2.0*T), N//2)
>>> import matplotlib.pyplot as plt
>>> plt.plot(xf, 2.0/N * np.abs(yf[0:N//2]))
>>> plt.grid()
>>> plt.show()
```

<img src="https://docs.scipy.org/doc/scipy/reference/_images/fft-1.png" alt="../_images/fft-1.png" style="zoom:80%;" />



푸리에 변환의 핵심은 **데이터 해석의 관점을 바꾸는 것**이다. 기존의 시간을 기준으로 보았던 파동을 주기 함수(빈도수)의 분포(주파수)로 본다. 시간 뿐만 아니라 공간의 경우에도 적용할 수 있는데, 이 때는 공간과 파수(wavenumber)가 서로의 쌍으로 된다. 심지어 시간과 공간 뿐만 아니라 다양한 분야에도 쓰일 수 있다. 한 예로 사진 압축에도 쓰일 수 있는데, 대표적인 압축 형태인 JPEG(Joint Photographic Experts Group)는 푸리에 변환의 일종인 DCT(Discrete Cosine Transform, 이산 코사인 변환)에 기반하고 있다.