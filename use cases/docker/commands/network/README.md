# Docker Network 활용하기

### 네트워크란?
* 컴퓨터들이 서로 cable로 연결하여 정보 및 데이터를 공유
* 네트워크의 핵심장비 = Switch

### Docker Network의 종류
* Bridge : ****DHCP기능****, 하나의 host 컴퓨터 내에서 여러 container들이 서로 통신,
  - 외부 -> 내부(container) : ****포트포워딩****
  - 내부(container) -> 외부 : ****NAT****
* Host : container가 host IP를 사용하는 것
  - container가 호스트 IP를 직접 사용, 외부에서 접속할 때 Host IP를 사용해 접속하므로 포트포워딩이 필요없음
  - host에 web service를 올리지 않고 container에 web server를 80 PORT로 열어 접속이 가능하다
* NULL : container들을 다른 컨테이너 또는 외부로 통신을 못하게 하는 것. -> ****container 고립****
* MACVLAN(ipvlan) : Host가 연결된 network IP를 사용하고, container들이 ****각각 다른 MAC address****를 갖는다
  - Host의 NIC에 여러개의 IP address를 지정하여 각 IP address마다 다른 MAC주소를 할당하여 container가 사용하도록 하는 것이다
  - 이를 위해 Host NIC에 promiscuous mode를 설정하여 하나의 물리적인 NIC에 여러 개의 논리적인 IP를 할당하도록 한다.
  - 이렇게 하면 각 contianer에는 Host가 사용하는 동일한 network 대역의 IP를 사용하게 된다.
* Overlay : 여러 Host에 분산되어 돌아가는 container간의 통신 [docker swarm]
* * *

### docker network 만들어서 container끼리 통신해보기
```
# docker network 만들기 (default : bridge)
docker network create mybridgenet
docker network create -d bridge mybridgenet
docker network create -d host [이름] ※ 1개만 가능
docker network create -d null [이름] ※ 1개만 가능
```
<img src="https://user-images.githubusercontent.com/87213815/179382280-a3a2ac36-9eaf-4dce-ba60-6a5597a07f1a.png" width="400" height="100">
* -d : driver

### bridge-utils 
```
# tool install
yum install bridge-utils -y

# bridge 확인
brctl show
```
<img src="https://user-images.githubusercontent.com/87213815/179382401-b28c6b5b-b16e-4973-8593-145c55bb3755.png" width="450" height="50">

### 만든 network로 container만들기
```
# bridge network
docker run -itd --name bcon10 --net bridge alpine
docker run -itd --name bcon20 --net bridge alpine

# mybridgenet network (새로만든 network)
docker run -itd --name mycon10 --net mybridgenet alpine
docker run -itd --name mycon20 --net mybridgenet alpine

# Host network
docker run -itd --name hcon10 --net host alpine
docker run -itd --name hcon20 --net host alpine

# NULL network
docker run -itd --name ncon10 --net none alpine
docker run -itd --name ncon20 --net none alpine
```
<img src="https://user-images.githubusercontent.com/87213815/179382772-d330b60b-e654-401e-9af8-cc25142c285b.png" width="600" height="200">
* -itd : -it + -d = 쉘로진입 + 백그라운드모드
* --name : container 이름
* --net : network 

```
# 각각 container IP 확인
# bridge network
docker exec -it bcon10 hostname -i
docker exec -it bcon20 hostname -i

# Host network
docker exec -it hcon10 hostname -i
docker exec -it hcon20 hostname -i

# mybridge network (새로만든 network)
docker exec -it mycon10 hostname -i
docker exec -it mycon20 hostname -i

# NULL network
docker exec -it ncon10 hostname -i
docker exec -it ncon20 hostname -i
```
<img src="https://user-images.githubusercontent.com/87213815/179383391-c32bbe3f-f149-4685-ba74-bb1ea3beee94.png" width="500" height="250">

* bridge , mybridgenet : 서로 다른 bridge이기 때문에 ***IP network 대역이 다르다. [brige : 172.17~, mybridgenet : 172.20~]***
* host , null : 최대 1개까지 만들 수 있어서 ***IP가 동일***

### container끼리 통신
```
# bridge (bcon10) - bridge (bcon20)
docker exec -it bcon10 ping 172.17.0.3

# bridge (bcon10) - mybridgenet (mycon10)
docker exec -it bcon10 ping 172.20.0.2

# bridge (bcon10) - host (hcon10)
docker exec -it bcon10 ping 10.0.2.10
```
<img src="https://user-images.githubusercontent.com/87213815/179383642-baded379-bc9b-4d48-ad7a-f8225d770e3e.png" width="600" height="300">

* host, 같은 network 는 통신 O

* 다른 network 는 통신 X
