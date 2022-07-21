# Adapter Bridge Network

<img src="https://user-images.githubusercontent.com/87213815/180209171-38e363ae-33fe-4087-9c73-73cbdf94f020.png" width="500" height="300">

### 내부 -> 외부 : NAT
### 외부 -> 내부 : Adapter Bridge
* 기존에 같은 포트를 쓸 때 vm에서 포트포워딩을하거나 docker에서 -p 80:80 과같은 명령어로 해야했음.

***192.168.56.1:80 과 192.168.56.1:8080 을 이젠 192.168.0.5 와 192.168.0.6 으로 접속 가능.***
* * *
## master, node1, node2, node3, node4 Setting
* 설정 → 네트워크 → 어댑터1: NAT  & 어댑터2: 어댑터에 브리지
<img src="https://user-images.githubusercontent.com/87213815/180221472-2a39061b-01f0-49c9-8c1e-23793b8ae2aa.png" width="500" height="250">
<img src="https://user-images.githubusercontent.com/87213815/180222206-46be9088-5f60-467d-8eb9-efef4744973e.png" width="400" height="250">
<img src="https://user-images.githubusercontent.com/87213815/180222280-50e38b2b-c323-4a44-b3ee-3ee39be888ad.png" width="400" height="250">

```
# ip 설정 바꾸기 static -> dhcp
vi /etc/sysconfig/network-scripts/ifcfg-enp0s3
:wq 저장 후
systemctl restart network
```

<img src="https://user-images.githubusercontent.com/87213815/180222738-f7ab24f7-ea0b-4ca9-9945-f2ecda244b02.png" width="600" height="300">

```
# network 확인
netstat -rn 
```

<img src="https://user-images.githubusercontent.com/87213815/180223184-6252bcdf-aebd-4949-9534-d6f036d30c1c.png" width="600" height="100">

```
# IP 확인
ifconfig
```
<img src="https://user-images.githubusercontent.com/87213815/180223189-fac0f4c3-60c5-470c-ba6f-3c13237157b9.png" width="600" height="400">

* * *
### ****master, node1, node2, node3, node4 전부 설정****
* * *

```
# 서로 다른 IP로 접속가능 
ssh [IP] -l root
```
<img src="https://user-images.githubusercontent.com/87213815/180225995-10593bcd-32bd-4cc3-8ba9-0876f213d1cb.png" width="600" height="300">

### Master 에서 작업
```
# IP가 바뀌었으므로 /etc/hosts 파일을 수정
vi /etc/hosts
```
<img src="https://user-images.githubusercontent.com/87213815/180227087-873fd43e-ee48-4983-ac61-a982eb9571ba.png" width="600" height="250">

```
# master의 /etc/hosts 파일을 node1, node2, node3, node4에 복사
scp /etc/hosts node1:/etc/hosts
scp /etc/hosts node2:/etc/hosts
scp /etc/hosts node3:/etc/hosts
scp /etc/hosts node4:/etc/hosts
```

## master, node1, node2, node3, node4 명령어를 실행하는 주체 외에 다 ssh copy
```
# ssh keygen
ssh-keygen

# node1, node2, node3, node4로 key복사
ssh-copy-id root@node1
ssh-copy-id root@node2
ssh-copy-id root@node3
ssh-copy-id root@node4
```
![화면 캡처 2022-07-21 224300](https://user-images.githubusercontent.com/87213815/180228518-2f67cff8-677e-47c2-9954-34f1febbc25c.png)

## 방화벽, selinux 끄기 -> nfs서버 초기설정
```
# 방화벽 상태확인
systemctl status firewalld

# 방화벽 끄기
systemctl disable firewalld --now
```
```
# selinux 파일 수정
vi /etc/sysconfig/selinux
```
![Untitled](https://user-images.githubusercontent.com/87213815/180229306-44a2f42d-9412-4a07-8f5c-6b547f6e62b2.png)

