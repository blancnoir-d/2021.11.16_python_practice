## **이상치**

단순히 정규화, 표준화를 하면 되는 거 아닌가 싶었는데 이상치에 대해 듣게 되어서 공부

⇒ 이상치란 다른 데이터보다 아주 작은 값이나 아주 큰 값을 말한다. 데이터를 분석할 때 이상치는 의사결정에 영향을 미칠 수 있다. 모형 구축에 있어 이상치는 그 빈도에 비해 아주 큰 영향력을 가지므로 정확한 모수(parameter) 추정에 어려움을 준다.그러나 이상치가 항상 의미없는 값이라고는 할 수 없으므로, 그 데이터에 대한 지식을 가지고 있는 전문가가 이상치에 대해 검토하는 것이 바람직하다

### **이상치 처리 방법**

이상치 처리 방법은 결측치와 유사하다. 제거와 치환 외에 분리하는 방법이 더 있을 뿐이다

① 제거

오타, 오류, 비상식적 반응과 같은 경우는 단순히 제거한다

② 치환

삭제가 어려운 경우에는 평균, 최빈값, 중앙값, 예측값 등으로 치환한다

단, 결측값의 경우와 같이 신뢰도 문제가 발생한다

③ 분리


독립변수가 충분히 세분되지 않은 경우 이상치가 발생할 수 있다

이러한 경우에는 변수를 세분하여 이상치를 분리한다

### 박스플롯을 이용해서 이상치 확인

![boxplot](https://user-images.githubusercontent.com/33194594/148229412-d48acca3-995f-4cf6-952a-b263cd1a177c.png)

### 다섯 수치 요약은 아래와 같다.

1. 최솟값 : 제 1사분위에서 1.5 IQR을 뺀 위치이다.
2. 제 1사분위(Q1) : 25%의 위치를 의미한다.
3. 제 2사분위(Q2) : 50%의 위치로 중앙값(median)을 의미한다.
4. 제 3사분위(Q3) : 75%의 위치를 의미한다.
5. 최댓값 : 제 3사분위에서 1.5 IQR을 더한 위치이다.

최솟값과 최댓값을 넘어가는 위치에 있는 값을 이상치(Outlier)라고 부른다.

### 이상치 제거하기

25%에 위치한 값을 구해줍니다.

`quantile_25 = np.quantile(train['fixed acidity'], 0.25)`

---

75%에 위치한 값을 구해줍니다.

`quantile_75 = np.quantile(train['fixed acidity'],0.75)`

---

IQR을 구해줍니다.

`IQR = quantile_75 - quantile_25`

---

quantile_25보다 1.5 * IQR 작은 값을 구해줍니다.

`minimum = quantile_25 - 1.5 * IQR`

---

quantile_75보다 1.5 * IQR 큰 값을 구해줍니다.

`maximum = quantile_75 + 1.5 * IQR`

---

minimum보다 크거나 같고, maximum보다 작거나 같은 값들만 뽑아냅니다.

`train2 = train[(minimum <= train['fixed acidity']) & (train['fixed acidity'] <= maximum)]`

출처 

[https://dacon.io/competitions/open/235698/talkboard/403815](https://dacon.io/competitions/open/235698/talkboard/403815)

[https://m.blog.naver.com/lingua/221909198917](https://m.blog.naver.com/lingua/221909198917)

실제로 써봄

```python
#IQR 구하기 
quantile_25 = np.quantile(df['Pregnancies'], 0.25)
quantile_75 = np.quantile(df['Pregnancies'], 0.75)
IQR = quantile_75-quantile_25
print(IQR)

#이상치 기준 
minimum = quantile_25 - 1.5*IQR
maximum = quantile_75 + 1.5*IQR

#이상치 제거 
df = df[(minimum <=df['Pregnancies']) & (df['Pregnancies']<=maximum)]
df
```

## 회귀 과제 진행 중에

- [데이터프레임 특정 행 삭제](https://www.notion.so/fed1c01a435e4d978731c431ae1908bf)

```python
df.drop(index=125)
```

- [특정 컬럼 삭제](https://www.notion.so/fed1c01a435e4d978731c431ae1908bf)

```python
df.drop(['컬럼명'], axis=1)
```

- df[’Age’].argmax()  /  df[’Age’].idxmax()

하위 버전에서는 argmax() → 이후 버전 idxmax()

최대값 인덱스

- 최소/최대값 인덱스
    - idxmin() : 최소값을 가지는 인덱스 레이블을 출력
    - idxmax() :최대값을 가지는 인덱스 레이블을 출력
        
        ```python
        #사용 방법
        result = df.idxmin(axis=0, skipna=True)
        result = df.idxmax(axis=0, skipna=True)
        ```
        
### 손실값, 정확도 확인하면서
![over](https://user-images.githubusercontent.com/33194594/148229748-c3774da5-e54d-4682-8ea5-4a17ffaaa64e.png)

- 과대적합
- 계속 정확도랑 수치가 너무 안정적이지가 않아서 Dropout도 줘보고 Earlystop도 해보는데 전혀 나아길 기미가 안보임
- val_loss가 계속 증가하는 형태

출처 : [https://lsjsj92.tistory.com/353](https://lsjsj92.tistory.com/353)

위의 글쓴이가 자문을 구해서 알아낸 방법은 학습률을 추가해 보았다
```python
sgd = keras.optimizers.SGD(lr = 0.01, decay = 1e-6, momentum = 0.9, nesterov=True)
```
나는 학습률이나 이런거 전혀 신경쓰지 않아서 => 추후 공부 필요

훨씬 나아짐 ㅠㅠ 
![editgg](https://user-images.githubusercontent.com/33194594/148230690-d159ccb8-41df-4604-be07-862f9dc2ceea.png)




bath size만 늘려도 이상하게 나오네 32일때 잘 나오고
split 할 때 random_state를 1로 주면 잘 나오고 10으로 val_loss 증가하고 => 알수가 없다...ㅠㅜ


- loss: 훈련 손실값
- acc: 훈련 정확도
- val_loss: 검증 손실값
- val_acc: 검증 정확도


과소적합 과대적합
![overunder](https://user-images.githubusercontent.com/33194594/148230499-cc13f854-68b5-4062-8d61-c657fcd0f9a7.png)

출처 : [https://ukb1og.tistory.com/28](https://ukb1og.tistory.com/28)


- [plt.legend()](https://www.notion.so/51bf0d53167c4ccfad376817584c3d06)

그래프 범례 추가 

```python
import numpy as np

import matplotlib.pyplot as plt

X = np.linspace(0, 100, 100)
Y1 = X**2
Y2 = X**2+5000

#범례 쓸때plot에 label 추가 
plt.plot(X,Y1, label='Y1')
plt.plot(X,Y2, label='Y2')

plt.legend()

plt.show()
```
