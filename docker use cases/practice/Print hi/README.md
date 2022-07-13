## 3초마다 hi 띄우는 container 만들기
```
# container 생성
docker run --name myrepeat -itd ubuntu bash -c "while true ; do echo hi ; sleep 3 ; done"
```

```
# container 쉘 진입
docker exec -it myrepeat bash
```

### 기타 명령어 
```
# container log 보기
docker logs [container 이름]

# shell 프로세스 log 확인
ps aux

# exited 상태인 container 한번에 시작
docker start $(docker ps -q -f status=exited)

# 실행중인 container 한번에 삭제
docker rm $(docker ps -aq) -f
```
