### Kafka-Server 구성하기

- java 설치

```python
sudo yum install -y java-1.8.0-openjdk-devel.x86_64
```

- 카프카 설치

공식 사이트 : [Apache Kafka](https://kafka.apache.org/downloads)
![kafkasite](https://user-images.githubusercontent.com/33194594/148387001-50630c8a-7746-4bd6-b95d-cc0be7590a47.png)


```python
wget https://dlcdn.apache.org/kafka/3.0.0/kafka_2.13-3.0.0.tgz
```
![download_kafka](https://user-images.githubusercontent.com/33194594/148387104-df5851b0-6c7a-4a96-b208-eb4190b4b47e.png)


- 압축 풀기 

```python
tar -xvf kafka_2.13-3.0.0.tgz
```

- 파일 확인

```python
ls -al 
```
![kafkafilecheck](https://user-images.githubusercontent.com/33194594/148387222-dcf0a3e7-b3a8-4d1f-90c0-fc29b0143205.png)

- 심볼릭 링크 설정 

```python
ln -s kafka_2.13-3.0.0 kafka
```


```python
[ec2-user@ip-172-31-38-253 ~]$ ln -s kafka_2.13-3.0.0 kafka
[ec2-user@ip-172-31-38-253 ~]$ cd kafka
[ec2-user@ip-172-31-38-253 kafka]$ pwd
/home/ec2-user/kafka
[ec2-user@ip-172-31-38-253 kafka]$ ls
bin  config  kafka_2.13-3.0.0  libs  LICENSE  licenses  NOTICE  site-docs
[ec2-user@ip-172-31-38-253 kafka]$
```

- 카프카로 이동해서 주키퍼 띄우기

```python
./bin/zookeeper-server-start.sh config/zookeeper.properties 
```

- 카프카서버 띄우기

```python
./bin/kafka-server-statr.sh config/server.properties 
```

- 2개의 데몬이 떠 있는지 확인하기
    - 9092  port : kafka
    - 2181 port :zookeeper

```python
sudo netstat -anp | egrep "9092|2181"
```



**netstat**(network statistics)는 전송 제어 프로토콜, 라우팅 테이블, 수많은 네트워크 인터페이스(네트워크 인터페이스 컨트롤러 또는 소프트웨어 정의 네트워크 인터페이스), 네트워크 프로토콜 통계를 위한 네트워크 연결을 보여주는 명령 줄 도구이다

리눅스에서 특정 포트가 리스닝 중인지 혹은 접속 중인지 확인하려면 다음과 같은 명령어를 실행하면된다.

```python
netstat -an | grep 포트번호
```

출처:[https://goni9071.tistory.com/272](https://goni9071.tistory.com/272)


9092 포트가 열려있어야 다른 서버에서 메시지를 받을 수 있음. producer가 메시지를 넘길 수 있음

- Kafka topic 생성
    
    프로듀서와 컨슈머가 공유할 토픽을 생성한다.
        

```python
#다른 예제 - hello_world 라는 이름의 토픽을 생성
#출처 https://gem1n1.tistory.com/121
$ {KAFKA_HOME}/bin/kafka-topics.sh --create --topic hello_world --zookeeper localhost:2181 --partitions 1 --replication-factor 1

#뭔지 모르겠지만.. 그래 해보면서 깨달아야지 
bin/kafka-topics.sh --create --topic twitter --partitions 1 --replication-factor 1 --bootstrap-server localhost:9092
```

토픽의 파티션, replication-factor(나중에 공부해볼것)는 늘릴 수도 있지만 개발 중 테스트 용도로 사용할 것이므로 최소로 하여 생성한다.

우선 생성해봄 
![topic](https://user-images.githubusercontent.com/33194594/148387662-21ab11e2-ab5f-42d8-9838-e19669374ba4.png)


- 토픽 리스트 확인

```python
bin/kafka-topics.sh --list --bootstrap-server localhost:9092
```
![topiclist](https://user-images.githubusercontent.com/33194594/148387715-8438463b-c2bc-444d-8999-4cd2ccc98ada.png)


- 메시지가 잘 쌓이는지 서버 한대에서 확인해보기

카프카에서는 메세지 생성하는 프로듀서와 컨슈머가 있음 

- 컨슈머 띄우기
프로듀서에서 메시지를 보내면 출력되는걸 확인할 수 있다 

```python
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic twitter --from-beginning
```

- 같은 서버 하나 더 띄움. 카프카 서버내에서 메시지를 보내보자 

- 카프카 프로듀서 실행

```python
#토픽 트위터에다가 보낼 메세지 
bin/kafka-console-producer.sh --bootstrap-server localhost:9092 --topic twitter
```


- producer 서버 구성

앞과 마찬가지로 yum update, java설치, kafka 설치, 링크설정까지 함 

그리고 IP

다른 서버이기 때문에 IP를 이용해야하는데 이때 AWS의 프라이빗IPv4주소를 써야한다. 퍼블릭으로 쓰면 안됨 내부통신이기때문에 퍼블릭을 통해서 프라이빗으로 가기 때문에 


```python
bin/kafka-console-producer.sh --bootstrap-server privateIP:9092 --topic twitter
```


- producer 빠져나오기

ctrl + d 


## Logstash 구성 및 twitter 연결

### producer에다가 Logstash 설치

- Java 설치

[https://www.elastic.co/guide/en/logstash/current/getting-started-with-logstash.html](https://www.elastic.co/guide/en/logstash/current/getting-started-with-logstash.html)

Logstash를 설치하기 위해서는 자바가 필요하고 버전은 이렇다

 

**Java (JVM) version**

Logstash requires one of these versions:

- Java 8
- Java 11
- Java 15 (see [Using JDK 15](https://www.elastic.co/guide/en/logstash/current/getting-started-with-logstash.html#jdk15-upgrade) for settings info)

앞에서 kafka 설치하면서 했으니까 안해도 됨 

최신 Apache Log4j 릴리스로 업그레이드하고 일부 취약성 스캐너에 대한 오탐지 문제를 해결하기 위해 Elasticsearch 및 Logstash의 새 버전인 7.16.2 및 6.8.22를 발표하게 된 것을 기쁘게 생각합니다. *[Reference] : 달소, [「서버포럼 - Apache Log4j2를 업그레이드하기 위한 Elasticsearch 및 Logstash의 7.16.2 및 6.8.22 릴리스 소개」 https://svrforum.com/?document_srl=125188&mid=itnews&act=dispBoardContent](https://svrforum.com/?document_srl=125188&mid=itnews&act=dispBoardContent).*

```python
wget https://artifacts.elastic.co/downloads/logstash/logstash-7.16.2.tar.gz
#안되넹 
```

공식 사이트 

[https://www.elastic.co/guide/en/logstash/current/installing-logstash.html](https://www.elastic.co/guide/en/logstash/current/installing-logstash.html)

```python
sudo rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
```

Add the following in your `/etc/yum.repos.d/` directory in a file with a `.repo` suffix, for example `logstash.repo`

:  `/etc/yum.repos.d/` 로 가서 .repo 파일을 생성해라 예를 들어서 logstash.repo

그리고 그 파일에 아래와 같은 내용을 넣고 저장 

```python
[logstash-7.x]
name=Elastic repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
```

명령어로 설치 

```python
sudo yum install logstash
```

![Logstashsuccess](https://user-images.githubusercontent.com/33194594/148388117-5c66f9e6-1434-418c-ac8a-55afbe172ded.png)


설치 성공~

Logstash를  접속정보, 펌프라는 파일을 구성하는데 어느위치에서 실행할 수 있도록 Path를 넣어줘야 한다 → 라는데 설치 방법부터 다르니까 좀 알아봐야겠네 



 공식 사이트에서 실행 방법

```python
sudo systemctl start logstash.service
```

