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
