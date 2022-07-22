# Data volume의 필요성
* Docker Data volume 은 container에 저장될 data를 container가 아닌 docker host에 저장하기위해 사용한다.
* container의 file system은 union file system이지만 ***data volume을 사용하면 docker host의 file system을 사용하므로 container를 사용할 떄마다 access 할 수있다.***

<center><img src="https://user-images.githubusercontent.com/87213815/179343759-666323a4-d6fe-497a-a8ea-8f223ae016d9.png" width="500" height="300"></center>

* * *
### docker volume 활용하여 웹에 원하는 메시지 띄우기
```
# docker volume 만들기
docker volume create convol

# host의 docker volume 보기
docker volume ls
```
![화면 캡처 2022-07-16 155429](https://user-images.githubusercontent.com/87213815/179343905-31f61f2f-fca5-4ca0-914f-e0ea204fe4e0.png)

****volume의 path 찾기****
```
# 만든 docker volume path 찾기
find / -name "convol"

# 만들어진 convol volume에 데이터 넣기
cd /var/lib/docker/volumes/convol/_data
echo "welcome docker world" > index.html
```
![화면 캡처 2022-07-16 163558](https://user-images.githubusercontent.com/87213815/179345294-5468a150-6f9e-4427-b778-bb326a494e9e.png)

```
# 만든 docker volume path 찾기 2
docker volume inspect convol
```
<img src="https://user-images.githubusercontent.com/87213815/179345925-11a660f0-ac1a-4598-91f5-d601e0e42074.png" width="600" height="200">

****continaer의 index.html 파일위치 탐색****
```
# 만들 웹 os docker container의 index.html 파일 위치 탐색
docker run -d --name myweb httpd
docker exec myweb find / -name "index.html"
```
<img src="https://user-images.githubusercontent.com/87213815/179344480-b0bb524d-84f7-4242-add1-f4909fd79141.png" width="600" height="150">

****docker volume 사용해서 container 만들기****
```
# docker container 만들기
docker run -d --name myweb -p 80:80 -v convol:/usr/local/apache2/htdocs httpd
```
![화면 캡처 2022-07-16 164813](https://user-images.githubusercontent.com/87213815/179345705-81f9b187-b5ac-4571-a41e-dee414328d4f.png)

#### 192.168.56.1 으로 결과확인
![output](https://user-images.githubusercontent.com/87213815/179345703-51a7ea96-e8d7-469e-b191-da0b58ed8478.png)

* * *
# volume 응용
### nfs서버로 volume 공유하기

## Environment: Master (nfs서버)
### nfs 서버 기초 : 폴더공유
```
# 최상위에 shared directory 만듬
mkdir /shared

# shared directory 에 welcome.msg 파일 만듬
touch /shared/welcome.msg

# 권한설정
chmod 777 /shared

# 공유된 파일 확인
ls -l /shared
```
<img src="https://user-images.githubusercontent.com/87213815/180441802-09bf4e56-73cb-4917-814e-7031bebf224a.png" width="400" height="100">

```
# 공유하는 파일 경로설정
vi /etc/exports
```
![Untitled](https://user-images.githubusercontent.com/87213815/180441501-e8514bdd-acaf-4d54-b405-8163d3383005.png)

```
# 적용
exportfs -a

# 현재 공유된 파일보기
exportfs -v

# nfs 시스템 재시작
systemctl restart nfs
```
<img src="https://user-images.githubusercontent.com/87213815/180441276-9c9ac8df-750f-4124-a728-4610f9fdaa59.png" width="1000" height="50">

## Environment: node1 (local)

```
#nfs-utils 다운

yum install nfs-utils -y

# shared mount확인 

showmount -e master

🚨 오류시 systemctl status firewalld 확인 -> 켜져있으면 systemctl enable firewalld 로 끄기 !!

```
<img src="https://user-images.githubusercontent.com/87213815/180444729-14be2924-3655-4b03-9b2a-fe6908cce882.png" width="300" height="50">

```
# local 서버에 /mnt 폴더에 master:/shared 폴더를 마운트
mount -t nfs master:/shared /mnt

# 잘 mount 되었는지 확인
ls -l /mnt

# mount 풀기
umount -a
```
![화면 캡처 2022-07-22 220417](https://user-images.githubusercontent.com/87213815/180445090-f7605275-8998-4818-b881-cb6ad15dad39.png)

```
# mount 확인
df -h
```
<img src="https://user-images.githubusercontent.com/87213815/180445466-d83fa4e6-e68d-4b9a-95af-c227668c2aec.png" width="600" height="200">

* * *

## Enviroment: Master (local)
```
# local에서 volume 만들기
docker volume create --name [volume이름] --driver local --opt type=[서버타입] --opt o=addr=[nfs서버주소],rw --opt device=:/[mount한 폴더]
docker volume create --name nfsvol --driver local --opt type=nfs --opt o=addr=192.168.0.32,rw --opt device=:/shared
```
![Untitled](https://user-images.githubusercontent.com/87213815/180447010-0aa81b46-9f14-400c-a9b8-7022196101c1.png)

```
# container 만들기
docker run -it --name con100 -v nfsvol:/data centos:7

# container 안에 들어가서 welcome.msg 확인
docker exec -it con100 bash

# 공유된 파일 확인
ls -l /data
```

![화면 캡처 2022-07-22 221725](https://user-images.githubusercontent.com/87213815/180447144-20d984c2-f4bd-4bff-9f3d-f5d82173c26c.png)

### 🚨 오류정리
![Untitled (1)](https://user-images.githubusercontent.com/87213815/180447282-3efcd5b7-f459-4558-8381-0c09f180e94d.png)

```
# Error response from daemon: error while mounting volume '/var/lib/docker/volumes/nfsvol/_data': failed to mount local volume: mount :/consharedvol:/var/lib/docker/volumes/nfsvol/_data, data: addr=10.0.2.12: no route to host 
문제 해결

# nfsvol 삭제
docker volume rm nfsvol

# --opt o=addr=[nfs서버 IP주소] 다시확인
docker volume create --name nfsvol --driver local --opt type=nfs --opt o=addr=192.168.0.32,rw --opt device=:/shared

# volume 확인
docker volume ls
```

### 💡 HTML 파일 volume 사용법
```
# 홈 디렉토리 확인
docker run -d --name myweb nginx
docker exec myweb find / -name "index.html"

# -v [volume이름]:[nginx 홈디렉토리] 
docker run -d --name web80 -p 80:80 -v webvol:/usr/share/nginx/html/ nginx
```

### 💡 실행중인 container volume 공유하기

```
# con-a 일반 alpine container 생성
docker run -itd --name con-a -v /root:/data alpine

# con-a와 volume을 공유한 con-b busybox container 생성
docker run -itd --name con-b --volumes-from con-a busybox
```
### 🚨 con-a 사망
```
# con-b 확인
docker exec -it con-b sh

cd data
ls -l
```
<img src="https://user-images.githubusercontent.com/87213815/180447871-edb3a0d2-3d84-4591-a9e5-795fd3190a1d.png" width="500" height="200">

### ✨ -v 여러개도 가능
```
mkdir test1 test2 test3
touch /test1/ha1.txt /test2/ha2.txt /test3/ha3.txt

# con100 -> con200 -> con300
docker run -itd --name con100 -v /test1:/data -v /test2:/secret -v /test3:/imsi alpine
docker exec -it con200 ls -l /test1
```
