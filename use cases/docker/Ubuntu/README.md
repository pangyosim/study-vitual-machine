### 사용하려는 Host에 docker 설치
```
curl -sSL http://get.docker.com | bash

#Docker 실행
systemctl enable docker --now

#Docker 버젼확인
docker version
```

# container에서 ubuntu 사용하기
```
# docker container 생성
docker run -itd --name myubuntu ubuntu
```
![캡처](https://user-images.githubusercontent.com/87213815/178260932-42c681e5-2f2e-4a49-8759-d3537485f609.PNG)
* -it : 터미널의 입력을 계속해서 container로 전달.
* -d : 백그라운드모드.
* --name [container이름] : 컨테이너 이름을 지정.
```
# container가 잘 만들어졌는지 확인
docker ps -a
```
![캡처](https://user-images.githubusercontent.com/87213815/178262037-cbaa70c2-b52b-4526-be63-2416062ee6b5.PNG)
* -a : all
```
# 만든 container로 이동
docker exec -it myubuntu /bin/bash
또는
docker exec -it myubuntu bash
```
![캡처](https://user-images.githubusercontent.com/87213815/178262263-b464b497-8085-4fa7-9895-1da22ee1c58c.PNG)
* -it : bash 쉘을 실행
```
# update
apt update

# ping tool 설치
apt install iputils-ping

# ping test
ping 8.8.8.8
```
![캡처](https://user-images.githubusercontent.com/87213815/178263465-81234873-b094-44cb-88b0-70b2da8c1a03.PNG)

## 기타 명령어 정리
```
# container 나가기 [not option -d]
Ctrl + p -> Ctrl + q : container 실행한 채로 나가기
Ctrl + d : 종료하고 나가기 
exit : 종료하고 나가기

# release version 확인
cat /etc/*-release

# kernel version 확인
uname -r
```

## 특징
### Host kernel version = container ubuntu kernel version
#### Host
![캡처](https://user-images.githubusercontent.com/87213815/178265117-de9f85d7-0b49-420e-92d2-873fa74ff9da.PNG)
#### Container ubuntu
![캡처](https://user-images.githubusercontent.com/87213815/178265309-4a3b9ad2-1079-4ee1-8341-8e3aa825abbd.PNG)
* * *
