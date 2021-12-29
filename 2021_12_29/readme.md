
- [df.to_numpy()](https://www.notion.so/fed1c01a435e4d978731c431ae1908bf),  df.values

데이터 프레임을 numpy Array로 변환 하는 함수 

## 손실 함수 정리

(출처 [https://didu-story.tistory.com/27](https://didu-story.tistory.com/27))

### 1 [binary_crossentropy(이항 교차 엔트로피)](https://www.notion.so/2f8b2147e98d44beb0072ee0e1956e62)

- y값이 (ex. 0, 1)인 이진 분류기를 훈련할 때 자주 사용되는 손실 함수(multi-label classification)
- 활성화 함수 : sigmoid사용(출력값이 0과 1 사이의 값)

### 2 [categorical_crossentropy(범주형 교차 엔트로피)](https://www.notion.so/2f8b2147e98d44beb0072ee0e1956e62)

- 레이블(y) 클래스가 2개 이상일 경우 사용. 즉 멀티 클래스 분류에 사용됨
- 활성화 함수 : sotfmax(모든 벡터 요소의 값은 0과 1 사이의 값이 나오고 모든 합이 1이 됨)
- 라벨이 (0,0,1,0,0)(0,1,0,0,0) 과 같이 one-hat encoding 된 형태로 제공될 때 사용 가능

### 3 [sparse_categorical_crossentropy](https://www.notion.so/2f8b2147e98d44beb0072ee0e1956e62)

- 범주형 교차 엔트로피와 동일하게 멀티 클래스 분류에 사용
- one-hot encoding된 상태일 필요 없이 정수 인코딩 된 상테에서 수행 가능
- 라벨이(1,2,3,4) 이런식으로 정수 형태일 때 사용!

### 4 [mse(평균 제곱 오차 손실 mean squared error)](https://www.notion.so/2f8b2147e98d44beb0072ee0e1956e62)

- 신경망의 출려과 타겟이 연속값이 회귀 문제에서 널리 사용하는 손실 함수
- 회귀 문제에 사용 될 수 있는 다른 손실 함수
    - MAE(평균 절대값 오차 Mean absolute error)
    - RMSE(평균 제곱근 오차 Root mean squared error)
- 예측과 타겟값의 차이를 제곱하여 평균한 값(모두 실수값으로 계산)
- MSE가 크다는 것은 평균 사이에 차이가 크다는 뜻 / MSE가 작다는 것은 데이터와 평균사이의 차이가 작다는 뜻  즉 MSE는 데이터가 평균으로부터 얼마나 떨어져 있나를 보여주는 손실 함수
- 연속형 데이터를 사용할 때 주로 사용(주식 가격 예측 등)

1_2 멀티 분류  - 인물 

- from IPython.display import Image

주피터 노트북 자체에서 이미지를 볼 수 있게 하는 모듈같다 

```python
from IPython.display import Image
Image(filename='test.png')
```

- tensorflow_datasets

방대한 데이터를 지원해주고 있다.  공개 데이터

설치

```python
pip install tensorflow_datasets
```

```python
import tensorflow_datasets as tfds
tfds.list_builders()#지원해주는 데이터 리스트 

# celeba의 정보중 이용할 데이터만 추출
celeb_a = tfds.load('celeb_a')
```

- imageio

간단하게 이미지를 보는데 용이하다. 

```python
pip install imageio
```

- from skimage.transform import resize

Scikit-image는 pillo보다 고급 기능을 제공하며 엔터프라이즈급 응용프로그램을 작성하는데 적합하다.

설치

```python
pip install scikit-image
```

- f[rom tensorflow.keras.utile import to_categorical](https://www.notion.so/2f8b2147e98d44beb0072ee0e1956e62)

원핫 인코딩을 해주는 함수

```python
from tensorflow.keras import to_categorical

train_male_labels = to_categorical(train_male_labels)
train_smile_labels = to_categorical(train_smile_labels)
test_male_labels = to_categorical(test_male_labels)
test_smile_labels = to_categorical(test_smile_labels)
```

- [np.split()](https://www.notion.so/N-dfd612bf4b724f0ea18dae89a897eaca)

하나의 배열을 여러개의 하위 배열로 분할할 수 있는 함수 

예

```python
np.hsplit(x, 3) 
#x 배열을 수평 축(열 방향, column-wise)으로 3개의 배열로 분할

출처: https://rfriend.tistory.com/359 [R, Python 분석과 프로그래밍의 친구 (by R Friend)]
```

```python
train_male_labels, train_smile_labels = np.split(train_labels, 2, axis=1)
test_male_labels, test_smile_labels = np.split(test_labels, 2, axis=1)

#원핫 인코딩
train_male_labels = to_categorical(train_male_labels)
train_smile_labels = to_categorical(train_smile_labels)
test_male_labels = to_categorical(test_male_labels)
test_smile_labels = to_categorical(test_smile_labels)
```

이건 y 데이터 안이 성별, 웃음에 관한 데이터가 있는데 성별만, 스마일만 으로 2개를 분리해서 만드는거 

- [np.concatenate([array,array...], axis=1)](https://www.notion.so/N-dfd612bf4b724f0ea18dae89a897eaca)

numpy.ndarray를 자유롭게 합칠 수 있는 concatenate 함수

b.T는 b 배열의 전치를 말하며,axis 값을 조절하여 어떤 축을 기준으로 배열을 합칠지 정할 수 있습니다.

```python
import numpy as np

a = np.array([[1, 2], [3, 4]])
b = np.array([[7, 8], [9, 10], [11, 12]])

print(np.concatenate((a, b), axis=0))
print(np.concatenate((a, b.T), axis=1))
print(np.concatenate((a, b), axis=None))
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2c61e6cf-59e2-4e84-8f8f-188c6e9c58d1/Untitled.png)

- [np.hstack()](https://www.notion.so/N-dfd612bf4b724f0ea18dae89a897eaca)

배열을 결합하는 함수 hstack()은 배열을 옆으로 결합할 때 사용한다. 세로로 결합할 때는 vstack()을 사용한다. 

예 

```python
import numpy as np

# 배열 준비
a = np.arange(10).reshape(2,5)
b = np.arange(15).reshape(3,5)
c = np.arange(8).reshape(2,4)
d = list(range(5))

# 배열 확인용
print("a")
print(a)
print("b")
print(b)
print("c")
print(c)
print("d")
print(d)

=>
a
[[0 1 2 3 4]
 [5 6 7 8 9]]

b
[[ 0 1 2 3 4]
 [ 5 6 7 8 9]
 [10 11 12 13 14]]

c
[[0 1 2 3]
 [4 5 6 7]]

d
[0, 1, 2, 3, 4]
```

np.hstack()

가로로 결합하는 hstack을 사용하는 경우에는 배열 행이 일치해야 합니다.  요소 개수인 열을 일치하지 않아도 됩니다. 변수 a와 c는 행이 2줄씩이기 때문에 가로로 결합이 가능합니다. 하지만 변수 a와 b를 hstack 사용해서 가로로 결합하려고 하면 에러가 발생합니다.

```python
h = np.hstack((a,c))

[[0 1 2 3 4 0 1 2 3]

[5 6 7 8 9 4 5 6 7]]
```

```python
plt.imshow(np.hstack(x_train[:5]))
plt.show()
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6d79ce53-69c2-4a43-ac95-b799432f051f/Untitled.png)

 - np.vstack()

**vstack** 사용할 때 주의점으로는 **요소(열) 개수가 일치**해야 합니다.

일치하지 않으면 결합이 안됩니다.

**배열의 행은 일치하지 않아도 상관없습니다.**

```python
e = np.vstack((a,b))

[[ 0 1 2 3 4]

[ 5 6 7 8 9]

[ 0 1 2 3 4]

[ 5 6 7 8 9]

[10 11 12 13 14]]

f = np.vstack((a,d))

[[0 1 2 3 4]

[5 6 7 8 9]

[0 1 2 3 4]]
```

- [pd.DataFrame(data, index, columns, dtype, copy)](https://www.notion.so/fed1c01a435e4d978731c431ae1908bf)

(출처 : [https://dsbook.tistory.com/12](https://dsbook.tistory.com/12))

데이터 프레임 생성 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8ccec926-7eaf-41e2-acda-9cd439e880c2/Untitled.png)

```python
data = [1,2,3,4,5]
pd.DataFrame(data)

pd.DataFrame(data, columns = ['Name','Age'])

pd.DataFrame(data, index=np.arange(4))
pd.DateFrame(data, index=['rank1', 'rank2', 'rank3', 'rank4'])
```

 - 딕셔너리로 DataFrame 생성하기

key값이 DataFrame의 column역할을 하고 value값이 DataFrame의data 역할을 한다

```python
data = {'a':100, 'b':[10,20,30,40]}
df = pd.DataFrame(data, index = np.range(4))
print(df)
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8a5d5ba0-9996-4df6-be9a-481244783391/Untitled.png)

-딕셔너리로 구성된 리스트로 DataFrame 생성하기

key값은 column, value값은 data의 역할을 하며 비어있는 부분은 NaN으로 채워진다.

```python
data = [{'a':1, 'b':2}, {'a':5, 'c':20}]
df = pd.DataFrame(data)
print(df)
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/47d47afd-b33f-4f10-8795-32602244a771/Untitled.png)

 여기서 DataFrame을 생성할 때 column의 이름을 지정하게 되면 column의 이름과 동일한 key의 value값만 data로서 역할을 한다. 

```python
df1 = pd.DataFrame(data, index = ['first','second'], columns=['a','b'])
df2 = pd.DataFrame(data, index = ['first','second'], columns=['a','b1'])
print(df1)
print(df2)
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3eebe589-65ed-4111-84f9-1dee82873e8a/Untitled.png)

Series로 DataFrame 생성하기

Series는 pandas로 생성할 수 있는 1차원 형태의 데이터이다. 각 Series의 index가 column 역할을 한다.

```python
a = pd.Series([100,200,300],['a','b','c'])
b = pd.Series([101,202,303],['a','b','k'])
c = pd.Series([110,220,330],['a','b','c'])
df = pd.DataFrame([a,b,c])
print(df)
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2d72160f-b10a-4b82-a414-03445637ff82/Untitled.png)

Series로 구성된 Dict로 DataFrame 생성하기

Dict의 value로 Series 형태가 들어갔다면 각 Series의 index는 DataFrame의 index 역할을 한다.

```python
d ={'one':pd.Seried([1,2,3],['a','b','c']), 'two':pd.Series([1,2,3,4],['a','b','c','d'])}
df = pd.DataFrame(d)
print(df)
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1def4cac-b666-4a78-a69b-d04c274a7169/Untitled.png)
