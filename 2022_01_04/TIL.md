# Kafka-Server 구성하기

- java 설치

```python
sudo yum install -y java-1.8.0-openjdk-devel.x86_6
```

- 카프카 설치

공식 사이트 : [Apache Kafka](https://kafka.apache.org/downloads)
```python
wget https://dlcdn.apache.org/kafka/3.0.0/kafka_2.13-3.0.0.tgz
```
  압축 풀기 

```python
tar -xvf kafka_2.13-3.0.0.tgz
```

파일 확인

```python
ls -al 
```

심볼릭 링크 설정 

```python
ln -s kafka_2.13-3.0.0 kafka
```

# Sequence to sequence model 
![s2s](https://user-images.githubusercontent.com/33194594/148064011-fa1f7ca5-2ff8-4598-ad04-4f5fad27051b.png)



# 정규화와 표준화의 차이 

정규화 (Normalization) : 정규화 할 값에서 최소값을 빼고 그 값을 최대값에서 최소값을 뺀 값으로 나눈 값. 0~1 범위를 갖게 되는 값들

![normalize](https://user-images.githubusercontent.com/33194594/148064209-b87780fc-de3e-465c-8c19-a449991a69cd.png)


표준화(Standardization) :표준화 할 값에서 평균값을 빼고 그 값을 표준편차로 나눈 값 . 표준화는 어떤 특성의 값들이 정규분포, 
즉 종모양의 분포를 따른다고 가정하고 값들을 0의 평균, 1의 표준편차를 갖도록 변환해주는 것. 표준화를 해주면 정규화처럼 특성값의 범위가 0과 1의 범위로 균일하게 바뀌지 않는다.

![standard](https://user-images.githubusercontent.com/33194594/148064298-e21759cd-2eff-43ac-829d-406b183bb378.png)

## 정규화와 표준화 중에 어떤 것이 더 나은가?

케이스 바이 케이스(case by case). 어떤 경우에는 정규화를 해줬을 때 더 좋은 성능을 낼 수 있고, 
어떤 경우에는 표준화가 더 나을 수도 있다. 따라서 둘 다 해보고 어느 것이 더 나은지 비교해봐야 한다. 
그리고 정규화나 표준화를 한 것과 하지 않은 것의 차이는 엄청나게 크기 때문에 꼭 해줘야 한다.


# Conv1d

주가 예측에서 windowing과 함께 Conv1D Layer가 쓰인것을 보고 찾아봄 

⇒ CNN은 일반적으로 이미지에서 계층적 특징 추출을 위해 사용된다. CNN의 이러한 장점을 활용하여 2차원 이미지가 아닌 1차원의 sequential 데이터에도 CNN이 사용된다. 
주어진 sequence data에서 중요한 정보를 추출해낼 수 있다. 1차원 CNN은 이미지가 아닌 시계열 분석 (time-series analysis)나 텍스트 분석을 하는데 주로 많이 사용된다. 
여기에서 1차원이라는 것은 합성곱을 위한 커널과 적용하는 데이터의 sequence가 1차원의 모양을 가진다는 것을 의미한다.

출처:[https://gmnam.tistory.com/274](https://gmnam.tistory.com/274)
