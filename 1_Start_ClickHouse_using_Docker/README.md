# 도커 서비스로 클릭하우스 데이터 베이스 시작하기

내가 사용할 수 있는 데이터베이스를 구축한다는 것은 비전공자인 나에게는 매우 어려운 일이었다. 따라서 회사에 입사하기 전에는 데이터베이스도 생소했고, SQL Query를 실제로 쓸 일이 거의 없었다. 

회사에서는 도커를 사용하고 있었는데.. 도커를 사용하면 보다 간단하게 SQL를 사용할 수 있는 환경을 구축할 수 있다. 

## 1. Clickhouse-Server
* [ClickHouse Server Docker Image](https://hub.docker.com/r/yandex/clickhouse-server/)

### ClickHouse Server 도커 이미지 다운로드
```bash
$ docker pull yandex/clickhouse-server
```

### ClickHouse Server 이미지 사용 방법
Container exposes 8123 port for HTTP interface and 9000 port for native client
```bash
$ mkdir $HOME/some_clickhouse_database # 마운트 시키고 싶은 디렉토리
$ docker run -d --name {원하는 컨테이너명} --ulimit nofile=262144:262144 --volume=$HOME/some_clickhouse_database:/var/lib/clickhouse -p 8123:8123 -p 9000:9000 yandex/clickhouse-server
```

ClickHouse DBMS를 사용할 수 있도록 되었다. Bash창을 통해 Query를 실행시켜도 되지만, 불편하고 익숙해지기 어렵다. 따라서 `Tabix`라는 SQL editor tool을 사용하려고 한다. 

## 2. Tabix
> Open source simple business intelligence application and sql editor tool for Clickhouse.
* [Tabix Docker Image](https://hub.docker.com/r/spoonest/clickhouse-tabix-web-client)
* [About Tabix](https://tabix.io/)
* [Install Tabix](https://tabix.io/doc/Install/)

### Tabix 도커 이미지 다운로드
```bash
$ docker pull spoonest/clickhouse-tabix-web-client
```

### Tabix 이미지 사용 방법
```bash
$ docker run --name {원하는 컨테이너명} -d -p 8080:80 spoonest/clickhouse-tabix-web-client
# docker run --name tabix -d -p 8080:80 spoonest/clickhouse-tabix-web-client
```

## 3. ClickHouse Server and Tabix 
환경셋팅을 마치고, ClickHouse Server 컨테이너와 Tabix 컨테이너가 실행되었는지 확인하고, 직접 사용해보자.

```bash
$ docker ps
```

실제로 컨테이너가 실행되었다고 하더라도 컨테이너가 완전히 실행되기까지는 시간이 조금 걸릴 수 있다. 

### ClickHouse Server 확인
ClickHouse Server를 사용할 수 있는지 확인하기 위해서는 웹을 띄우고 url 창에 다음을 입력하고, OK가 뜨는지 확인해보자.
```
http://localhost:8123/
```
![](2020-07-03-01-38-21.png)

### Tabix 확인
Url 창에 다음을 입력한다. `Name *`에 원하는 이름을 입력하고 `http://host:port *`에 http://localhost:8123만 입력해주면 된다.
```
http://localhost:8080/
```
![](2020-07-03-01-40-41.png)


이것으로 Tabix라는 UI를 통해 SQL 쿼리를 실행할 수 있다. 
![](2020-07-03-01-43-19.png)


```
# https://www.quora.com/What-is-the-difference-between-SQL-and-a-query#:~:text=SQL%20%3A%20Structured%20Query%20Language%20(SQL,of%20rows%20from%20data%20base.
SQL : Structured Query Language (SQL) is a standard computer Programming language for relational database management and data manipulation.

SQL is used to query, insert, update and modify data.

Query : A query is a single SQL statement that does Select, Update, Insert or Delete of rows from data base.
```












참고
- [데이터베이스 - 서버와 클라이언트](https://server-talk.tistory.com/276)
- [도커란](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)
- [도커 컨테이너와 데이터베이스](https://this-programmer.com/entry/%EA%B3%BC%EC%97%B0-%EB%8F%84%EC%BB%A4Docker-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88%EB%A5%BC-%ED%86%B5%ED%95%B4-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4%EB%A5%BC-%EC%9A%B4%EC%98%81%ED%95%98%EB%8A%94-%EA%B2%8C-%EC%A2%8B%EC%9D%80-%EB%B0%A9%EB%B2%95%EC%9D%BC%EA%B9%8C)


