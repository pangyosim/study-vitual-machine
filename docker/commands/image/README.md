# Docker image 다루기
### docker image 압축하기
* 만약 internet이 안되는 local이 있다면 docker pull이 어려울 때 사용.
```
# mysql image를 .tar로 압축하고 저장
docker image save mysql > mysql.tar
```
<img src="https://user-images.githubusercontent.com/87213815/179733592-e6baac4f-aca2-45cf-8912-08f4c28dee25.png" width="600" height="300">
* > :  파일을 저장하거나 내용을 바꿀 때 사용.

```
# 저장된 이미지를 node1:/root 파일 경로로 복사
scp mysql.tar node1:/root/
```
![화면 캡처 2022-07-19 195445](https://user-images.githubusercontent.com/87213815/179734037-44087623-5016-46ac-916f-a5aec01c3806.png)

```
# node1에서 이미지 파일 로드하기
docker image load -i mysql.tar

# 이미지확인
docker images
```
<img src="https://user-images.githubusercontent.com/87213815/179735161-22fbed44-dbc0-4f2d-8cd4-2a2c04cc3f84.png" width="600" height="300">

```
#image history 확인
docker history mysql
```
![화면 캡처 2022-07-19 200214](https://user-images.githubusercontent.com/87213815/179735323-11c07772-6fb1-404a-ba7d-856aa1f1e6f9.png)
* * *
