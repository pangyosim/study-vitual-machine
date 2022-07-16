### 사용하려는 Host에 docker 설치
```
curl -sSL http://get.docker.com | bash

#Docker 실행
systemctl enable docker --now

#Docker 버젼확인
docker version
```

# container에서 mysql database 운영하기
```
# container 만들기
docker run --name mysqlcon -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=password mysql
```
![화면 캡처 2022-07-12 215209](https://user-images.githubusercontent.com/87213815/178494273-7098aaaf-3394-4fe1-b46d-6906adeee4d4.png)
* --name : container 이름
* -d : 백그라운드모드
* -p [호스트포트]:[이미지포트]
* -e : 환경변수 설정

```
# mysqlcon bash에 진입
docker exec -it mysqlcon bash
```
![화면 캡처 2022-07-12 215444](https://user-images.githubusercontent.com/87213815/178494785-c50f9df4-78ec-4d05-ba1a-121c38d9d49d.png)
* -it : 터미널의 입력을 계속해서 container로 전달

```
# mysql database에 로그인 (container 생성시 환경변수 설정대로 PW: password) 
mysql -u root -p
```
![화면 캡처 2022-07-12 215604](https://user-images.githubusercontent.com/87213815/178495025-f442d609-569e-42a3-bcfc-43d3bdd61df6.png)
* -u : username
* -p : password

### 기타 명령어
```
# database 보기
show databases;

# table 보기
show tables;

# database 전환 (form :use [database])
use mysql

# database 선택
select database();

# version 보기
select version();

# 현재시간보기
select now();

# 나가기
exit
```
* * *
