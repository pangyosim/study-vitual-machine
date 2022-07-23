# Ansible로 container관리
* 중앙에서 패키지 설치 및 구성가능
* 한 대의 컴퓨터에서만 ansible 관리
```
# Ansible 설치 (NFS SERVER 설치된곳)
yum install ansible -y
```
![Untitled](https://user-images.githubusercontent.com/87213815/180602236-442efac6-e48f-4f06-8de8-ee15bcb8f5ee.png)
![Untitled](https://user-images.githubusercontent.com/87213815/180602244-5eef8f8f-31ad-45f3-b184-57ba881c68e4.png)

```
# ansible host 파일 수정
vi /etc/ansible/hosts

# 맨 아래 작업할 곳 추가
[servers]
master
node2
```
![화면 캡처 2022-07-23 201834](https://user-images.githubusercontent.com/87213815/180602853-071010b8-08de-4a95-82ce-b2e7f6d164e9.png)

```
# ping 확인
ansible servers -m ping

# ping 확인 비밀번호입력
ansible servers -m ping -k
```
![화면 캡처 2022-07-23 201752](https://user-images.githubusercontent.com/87213815/180602835-ef5fce07-7404-4220-9fb0-82b90a9fecaf.png)

```
# master, node2 동시 container 만들기
ansible servers -m shell -a "docker run -itd --name con100 -v convol:/data alpine"
```
![화면 캡처 2022-07-23 202359](https://user-images.githubusercontent.com/87213815/180603010-8e1dde31-fbe6-40b9-aed2-b41a4446a380.png)

```
# master, node2 container 확인
ansible servers -m shell -a "docker ps -a"
```
![화면 캡처 2022-07-23 202501](https://user-images.githubusercontent.com/87213815/180603027-68e4363d-8196-488d-9a84-b0b0276b38c3.png)
![화면 캡처 2022-07-23 202837](https://user-images.githubusercontent.com/87213815/180603111-7ef3e1fe-6192-408c-b754-56892fb2b2a2.png)
![화면 캡처 2022-07-23 202851](https://user-images.githubusercontent.com/87213815/180603113-8024c176-1098-4748-a6ed-a454a9db84b9.png)

```
# volume data 확인
ansible servers -m shell -a "docker exec con100 ls -l /data"
```

```
# 환경변수룰 이용해서 운영
ansible servers -m shell -a "docker exec con100 touch /data/${HOSTNAME}-good.news"
```

