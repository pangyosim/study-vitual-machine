### 사용하려는 Host에 docker 설치
```
curl -sSL http://get.docker.com | bash

#Docker 실행
systemctl enable docker --now

#Docker 버젼확인
docker version
```
* * *
# 호스트IP에서 게임 실행하기
```
# curl 명령어를 사용해서 IP주소에 있는 .sh 스크립트 파일을 bash로 실행
curl http://down.cloudshell.kr/docker/supermario.sh | bash
```
![화면 캡처 2022-07-12 195558](https://user-images.githubusercontent.com/87213815/178474829-cde95419-bcec-4141-91c1-4d5375e8002c.png)
### supermario.sh 파일
```
#!/bin/bash
docker run --name supermariocon -d -p 80:8080 pengbai/docker-supermario
```
* \#!/bin/bash : 스크립트파일 형식
* --name : container 이름
* -d : 백그라운드모드
* -p [호스트포트]:[이미지포트] 
### 호스트IP 192.168.56.1:80 으로 접속해서 확인
![화면 캡처 2022-07-12 200000](https://user-images.githubusercontent.com/87213815/178475403-f0c1250a-0dd4-47a3-a366-ed30f14bfcfd.png)
