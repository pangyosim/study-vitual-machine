# Docker Image 생성해서 Dockerhub에 올리기

### image만들 mycentos container 생성
```
docker run -itd --name mycentos centos:7
docker exec -it mycentos bash
```

### 필요한 패키지 다운로드
```
# bind-utils 다운로드
yum install bind-utils -y

# epel-release 다운로드
yum install epel-release -y

# tree 다운로드
yum install tree -y
```

### IMAGE 데이터를 만든다
```
# myimage 폴더생성
mkdir /myimage

# myimage 폴더에 welcome.msg 파일생성
touch /myimage/welcome.msg

# welcome.msg에 내용입력
echo “Hi, This is myimage” > /myimage/welcome.msg

# container에서 나가기
Ctrl +d 
```
![화면 캡처 2022-07-18 204214](https://user-images.githubusercontent.com/87213815/179503994-8983d3e6-3a0a-4b47-bf97-e432b7aed3a1.png)

```
# centos 중지 필수
docker stop mycentos
```
![화면 캡처 2022-07-18 204547](https://user-images.githubusercontent.com/87213815/179504522-4f646f5c-8738-423f-a0a9-0c3cc6ad4195.png)

### dockerhub에 올리기
```
# dockerhub에 commit
docker commit mycentos spg9468/centosimage

# image가 생긴 것을 확인
docker images
```
<img src="https://user-images.githubusercontent.com/87213815/179507919-96c03a66-90c0-402a-ac85-9c4c84299e36.png" width="450" height="300">

```
# dockerhub에 로그인
docker login -u [ID]

# dockerhub에 push
docker push [push image]
```
<img src="https://user-images.githubusercontent.com/87213815/179506920-2356d15c-4719-4c3a-9023-f9bbfac6fd47.png" width="400" height="200">

* login -u : userID

### dockerhub 사이트에 로그인해서 확인
https://hub.docker.com/
![Untitled](https://user-images.githubusercontent.com/87213815/179507495-5cfdcc68-f9a4-470a-8234-01195dcf1c02.png)

