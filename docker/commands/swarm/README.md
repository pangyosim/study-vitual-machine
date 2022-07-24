# Docker Swarm
![Untitled](https://user-images.githubusercontent.com/87213815/180646403-8598ebc8-ecd8-465d-9fd4-175ce038ba6d.png)

## Cluster 만들기
```
# docker
docker swarm init --advertise-addr 192.168.0.5

# swarm join
docker swarm join --token SWMTKN-1-1crk8bosnrnoimba6nm8vcjnum0a7sxnyrcqsjmwrut9uoiud2-6h8gon76ibosip45cs23lp0lq 192.168.0.5:2377

# swarm 삭제
docker swarm leave

# swarm node 확인
docker node ls
```
![Untitled](https://user-images.githubusercontent.com/87213815/180646795-3653b4dc-bf66-4e95-a941-60d4948c378a.png)

## Service 생성하기
```
# 웹 서비스 생성
docker service create --name myweb --replicas 3 -p 80:80 nginx
```
* --replicas [ ] : 만들 container 갯수
```
# docker 서비스 확인
docker service ls

# docker myweb 서비스 확인
docker service ps myweb
```
```
# docker service를 한개로 줄임
docker service scale myweb=1
[master에서 실행중]

# docker service를 5개로 설정
docker service scale myweb=5
```
