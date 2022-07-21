## 3초마다 hi 띄우는 container 만들기
```
# container 생성
docker run --name myrepeat -itd ubuntu bash -c "while true ; do echo hi ; sleep 3 ; done"
```
![화면 캡처 2022-07-15 205223](https://user-images.githubusercontent.com/87213815/179217936-a59493a6-1fd9-4482-8865-7e5ef9016883.png)
* --name : container 이름
* -itd : 쉘진입 + 백그라운드모드
* -c : 메모리 영역 사용

```
# container 쉘 진입
docker exec -it myrepeat bash

# shell 프로세스 log 확인
ps aux
```
![화면 캡처 2022-07-15 205338](https://user-images.githubusercontent.com/87213815/179218073-5599a3ad-bc95-4577-870c-a7eb8eba3424.png)


### 기타 명령어 
```
# container log 보기
docker logs [container 이름]

# exited 상태인 container 한번에 시작
docker start $(docker ps -q -f status=exited)

# 실행중인 container 한번에 삭제
docker rm $(docker ps -aq) -f
```
