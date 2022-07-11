# 같은 IP에서 다른 PORT로 nginx, apache(httpd) container 만들기
### Oracle VM Virtual Box 포트포워딩
#### - 파일 -> 환경설정
![화면 캡처 2022-07-11 220748](https://user-images.githubusercontent.com/87213815/178271310-3400ebfe-fa32-4f67-a33c-3169a96f4109.png)
#### - 네트워크 -> NatNetwork -> 포트포워딩
![화면 캡처 2022-07-11 220837](https://user-images.githubusercontent.com/87213815/178271448-d82327ca-d2c5-4f7f-97d3-a7963a351b90.png)
#### - 기존에 있던 22포트 복사 후 포트수정 nginx : PORT 80 apache(httpd) : PORT 8080
![화면 캡처 2022-07-11 220908](https://user-images.githubusercontent.com/87213815/178271526-cd924f67-b034-44d8-93c2-f4e44482eda6.png)
* * *

### nginx
```
# nginx container 만들기
docker run --name mynginx -d -p 80:80 nginx
```
![화면 캡처 2022-07-11 222002](https://user-images.githubusercontent.com/87213815/178275010-ef033600-59d7-43fc-a25c-665a994f39a6.png)
* -d : 백그라운드모드.
* -p [Host포트]:[guest포트] : 포트설정
```
# container nginx 확인
curl localhost:80
```
![화면 캡처 2022-07-11 222359](https://user-images.githubusercontent.com/87213815/178276389-93523996-1bfc-43e1-a269-3029d83b626f.png)

#### container nginx를 인터넷에서 확인
- URL 주소 -> 192.168.56.1:80

![화면 캡처 2022-07-11 222332](https://user-images.githubusercontent.com/87213815/178276771-865650dd-ba28-402f-8f9a-21167a754064.png)

### index.html 내용 수정하기
```
# mynginx container bash로 이동
docker exec -it mynginx bash
```
![화면 캡처 2022-07-11 223850](https://user-images.githubusercontent.com/87213815/178277331-7d4e0889-23d4-43a6-a000-19c0ff7fe3cf.png)
```
# index.html이 있는 경로 찾기
find / -name "index.html"
```
![화면 캡처 2022-07-11 224009](https://user-images.githubusercontent.com/87213815/178278272-8dd4801f-39c3-4064-8410-fd28bb8bcd5a.png)
```
# "Welcome to Docker World"를 index.html에 입력
echo "Welcome to Docker World" > /usr/share/nginx/html/index.html
```
![화면 캡처 2022-07-11 224450](https://user-images.githubusercontent.com/87213815/178278628-00ac1657-91e7-4f31-94d2-3ef463b1e138.png)
* echo "" > [file] : "" 내용을 file에 입력
```
# 수정된 container nginx 확인
curl localhost:80
```
![화면 캡처 2022-07-11 224642](https://user-images.githubusercontent.com/87213815/178278840-2600f19d-9e59-4bf2-b332-6d075b265047.png)
* localhost = 172.17.0.2
  ```
  # localhost 주소 찾기
  enviroment : master
  docker exec -it mynginx hostname -I
  
  enviroment : bash
  hostname -I
  ```
  ![화면 캡처 2022-07-11 225206](https://user-images.githubusercontent.com/87213815/178279849-070cf8dd-850c-4fd0-b012-5c245fc6b0c0.png)

### 수정된 container nginx를 인터넷에서 확인
- URL 주소 -> 192.168.56.1:8080

![화면 캡처 2022-07-11 223752](https://user-images.githubusercontent.com/87213815/178279106-dadd478a-7c54-4dd4-b3c2-dd0f98879e51.png)

* * *
### apache(httpd)
```
# apache(httpd) container 만들기
docker run --name myapache -d -p 8080:80 httpd
```
![httpd](https://user-images.githubusercontent.com/87213815/178280595-2017a0d5-4d67-4173-a0a0-172e23d9cb70.png)
* -d : 백그라운드모드.
* -p [Host포트]:[guest포트] : 포트설정
```
# container apache(httpd) 확인
curl localhost:8080
```
![화면 캡처 2022-07-11 222614](https://user-images.githubusercontent.com/87213815/178291275-d263d50a-f4f2-4a21-b39c-1728918c266b.png)
* localhost = 172.17.0.3
  ```
  # localhost 주소 찾기
  enviroment : master
  docker exec -it myapache hostname -I
  
  enviroment : bash
  hostname -I
  ```
  ![화면 캡처 2022-07-11 235918](https://user-images.githubusercontent.com/87213815/178294502-b4152942-29d4-4826-aa45-859c9ae31261.png)

### container apache(httpd)를 인터넷에서 확인
- URL 주소 -> 192.168.56.1:8080

![화면 캡처 2022-07-11 222637](https://user-images.githubusercontent.com/87213815/178293936-ebcdfffb-0c4e-46f4-b43d-fa50f2cf41a6.png)


