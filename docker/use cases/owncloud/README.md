### 사용하려는 Host에 docker 설치
```
curl -sSL http://get.docker.com | bash

#Docker 실행
systemctl enable docker --now

#Docker 버젼확인
docker version
```
* * *
# owncloud storage 운영가능
```
# owncloud container 만들기
docker run --name owncloud -d -e OWNCLOUD_DOMAIN=localhost:8080 -p 8080:8080 --restart=always owncloud/server
```
* --name : container 이름
* -d : 백그라운드모드
* -e : 환경변수 설정
* -p [호스트포트]:[이미지포트]
* --restart=always : docker를 키고 끌 때 항상 같이 키고 끄게하는
### 192.168.56.1:8080 실행해서 이용 (초기 ID: admin PW: admin)
![화면 캡처 2022-07-12 214522](https://user-images.githubusercontent.com/87213815/178492882-06a040a3-bcf0-42eb-bc44-a73b8a87ea12.png)
