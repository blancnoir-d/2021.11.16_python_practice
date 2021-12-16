## 텐서플로어

[Guide to the Sequential model - Keras Documentation](https://keras.io/ko/getting-started/sequential-model-guide/)

- keras.Sequential모델

`Sequential` 모델은 레이어를 선형으로 연결하여 구성합니다. 레이어 인스턴스를 생성자에게 넘겨줌으로써 `Sequential` 모델을 구성할 수 있습니다.

```
from keras.modelsimport Sequential
from keras.layersimport Dense, Activation

model = Sequential([
    Dense(32, input_shape=(784,)),
    Activation('relu'),
    Dense(10),
    Activation('softmax'),
])

```

또한, `.add()` 메소드를 통해서 쉽게 레이어를 추가할 수 있습니다.

```
model = Sequential()
model.add(Dense(32, input_dim=784))
model.add(Activation('relu'))
```

- model.compile

**컴파일**

모델을 학습시키기 이전에, `compile` 메소드를 통해서 학습 방식에 대한 환경설정을 해야 합니다. 다음의 세 개의 인자를 입력으로 받습니다.

- 정규화기 (optimizer). `rmsprp`나 `adagrad`와 같은 기존의 정규화기에 대한 문자열 식별자 또는 `Optimizer` 클래스의 인스턴스를 사용할 수 있습니다. 참고: [정규화기](https://keras.io/optimizers)
- 손실 함수 (loss function). 모델이 최적화에 사용되는 목적 함수입니다. `categorical_crossentropy` 또는 `mse`와 같은 기존의 손실 함수의 문자열 식별자 또는 목적 함수를 사용할 수 있습니다. 참고: [손실](https://keras.io/losses)
- 기준(metric) 리스트. 분류 문제에 대해서는 `metrics=['accuracy']`로 설정합니다. 기준은 문자열 식별자 또는 사용자 정의 기준 함수를 사용할 수 있습니다.

```
# For a multi-class classification problem
model.compile(optimizer='rmsprop',
              loss='categorical_crossentropy',
              metrics=['accuracy'])

# For a binary classification problem
model.compile(optimizer='rmsprop',
              loss='binary_crossentropy',
              metrics=['accuracy'])

# For a mean squared error regression problem
model.compile(optimizer='rmsprop',
              loss='mse')

# For custom metricsimport keras.backendas K

defmean_pred(y_true, y_pred):
return K.mean(y_pred)

model.compile(optimizer='rmsprop',
              loss='binary_crossentropy',
              metrics=['accuracy', mean_pred])
```

```python
import tensorflow as tf
from tensorflow import keras
import numpy as np

tf.random.set_seed(0)

model = keras.Sequential([keras.layers.Dense(units=3, input_shape=[1])])
model.compile(loss='mse', optimizer='SGD')
```

손실 함수로 ‘mse’를, 옵티마이저로 ‘SGD’을 지정

- tf.constant

[[tensorflow] tf.constant, tf.Variable, tf.get_variable](https://chan-lab.tistory.com/7)

**constant**는 상수를 뜻합니다. 수학에서 상수는 변수와 구분되는 개념으로, 변하지 않고 고정된 값을 의미합니다. 텐서플로에서 tf.constant() 함수를 통해 상수 텐서를 만들 수 있습니다.

**Variable**은 변수를 뜻합니다. 변수는 수학에서 투입값에 따라 변하는 숫자를 의미하지만, 텐서플로의 Variable은 Constant와 큰 차이가 없습니다.

### Rank

TensorFlow 시스템에서, tensor는 *rank*라는 차원 단위로 표현된다. Tensor rank는 행렬의 rank와 다르다. Tensor rank(*order*, *degree*, *-n_dimension* 으로도 언급됨)는 tensor의 차원수다. 예를 들어, 아래 tensor(Python 리스트로 정의)의 rank는 2다.

```
t = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
```
