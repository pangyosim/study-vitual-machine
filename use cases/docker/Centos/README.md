### 사용하려는 Host에 docker 설치
```
curl -sSL http://get.docker.com | bash

#Docker 실행
systemctl enable docker --now

#Docker 버젼확인
docker version
```

# container에서 centos 사용하기
```
# docker container 생성
docker run -itd --name mycentos centos:7
```
![캡처](https://user-images.githubusercontent.com/87213815/178266172-7b50193a-6da5-4489-8c01-748892b0943d.PNG)
* -it : 터미널의 입력을 계속해서 container로 전달.
* -d : 백그라운드모드.
* --name [container이름] : 컨테이너 이름을 지정.
```
# container가 잘 만들어졌는지 확인
docker ps -a
```
![캡처](https://user-images.githubusercontent.com/87213815/178266261-a62e6878-eea2-44d6-a888-bbfce912927a.PNG)
* -a : all
```
# 만든 container로 이동
docker exec -it mycentos /bin/bash
또는
docker exec -it mycentos bash
```
![캡처](https://user-images.githubusercontent.com/87213815/178266630-05857039-e1ac-44a7-ad59-51b2aa569b43.PNG)
* -it : bash 쉘을 실행
```
# ping test
ping 8.8.8.8
```
![캡처](https://user-images.githubusercontent.com/87213815/178266852-47cb34e5-ccc4-47de-999f-e94d71b78903.PNG)
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
![캡처](https://user-images.githubusercontent.com/87213815/178267130-9ea3bd1f-dd13-4fdf-b13c-f516e9f5bb6d.PNG)
#### Container centos
![캡처](https://user-images.githubusercontent.com/87213815/178267011-6cf97823-1b11-4c0c-aafa-a4622521c094.PNG)
* * *
