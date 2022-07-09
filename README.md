## Docker 환경만들기

* Oracle VM VirtualBox(v6.1.34)을 다운로드하고 실행.

https://www.virtualbox.org/wiki/Downloads

* mobaXterm 사이트에서 다운로드하고 실행.

https://moba.softonic.kr/

* centos 사이트에서 CentOS-7-x86_64-Minimal-2009.iso 다운.

http://ftp.kaist.ac.kr/CentOS/7.9.2009/isos/x86_64/

### Oracle VM VirtualBox에서 새로운 리눅스 가상환경(master)을 구축.
* 메모리크기: 1024MB
* VDI(VirtualBox 디스크 이미지)
* 파일 위치 및 크기 20GB

### master centos Settings
* master -> 설정 -> 시스템 플로피 체크 해제 & 포인팅장치 : USB 태블릿
* 저장소 컨트롤러:IDE (비어있음) -> 디스크 -> 디스크 파일 선택 -> CentOS-7-x86_64-Minimal-2009.iso 
* 네트워크 -> NAT 네트워크
* 시작 -> 한국어 > 시간대, 네크워크 ON
* root계정으로 로그인 -> ping [DNS] -> yum install epel-release -y -> yum repolist
* vi /etc/sysconfing/network-scripts/ifcfg-enp0s3
  
  수정
  - BOOTPROTO=static
  - ONBOOT=yes
  
  추가 **(POWERSHELL 열어서 ipconfig 명령어로 확인)**
  - IPADDR= [원하는 IP주소]
  - NETMASK= 255.255.255.0
  - GATEWAY= [원하는 GATEWAY]
  - DNS1= [DNS서버]
* systemctl restart network : 서버 재시작
* hostname -I : IP가 설정한대로 바뀌었는지 확인
* ping [IP주소] : 네트워크 확인
* yum -y install net-tools wget elinks bind-utils nfs-utils traceroute tree : 필요한 tools install
* yum update -y : 전체 tools update
* echo "master" > /etc/hostname : hostname을 master로 수정
* reboot 명령어 입력 후 hostname이 바뀌었는지 확인

### node1, node2, node3, node4 복제
* master 종료 필수
* node1, node2, node3, node4 복제 -> MAC주소 정책(P) : 모든 네트워크 어댑터의 새 MAC 주소 생성

  - node1&#126;4 각각 IP: 10.0.2.11&#126;14   
  #### From MobaXterm enviroment (node1,node2,node3,node4)   
  
      hostname 이름 변경
      - VM Virtualbox node ON -> ssh root@[node1 IP]
      - echo "node1" > /etc/hostname   
  
      node IP주소 변경 
      - vi /etc/sysconfig/network-scripts/ifcfg-enp0s3 
      - IPADDR=[각 node의 IP] 
 
 ### node ssh Settings
 #### From master enviroment
 - vi /etc/hosts 파일 수정 
  
 <img src="https://user-images.githubusercontent.com/87213815/178110807-3d8ffdc8-ba49-4e5d-ac79-2a77c0fe66c2.jpg" width="450px" height="300px" title="px(픽셀) 크기 설정" alt="RubberDuck"></img><br/>
 
  #### node1, node2, node3, node4 ssh keygen
  
  ```
  scp /etc/hosts node1:/etc/hosts 
  ssh-keygen
  ssh-copy-id root@node1
  ```
  *****ssh node1 입력 -> node1로 localhost가 전환되면 성공!!*****
*****
