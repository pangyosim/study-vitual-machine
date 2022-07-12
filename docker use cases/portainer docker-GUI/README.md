### 사용하려는 Host에 docker 설치
```
curl -sSL http://get.docker.com | bash

#Docker 실행
systemctl enable docker --now

#Docker 버젼확인
docker version
```
# portainer (docker GUI) 서비스 
### portainer docker volume 생성
```
# portainer_data 라는 volume 생성
docker volume create portainer_data
```
![화면 캡처 2022-07-12 201154](https://user-images.githubusercontent.com/87213815/178477129-d67a6683-3630-461d-b08b-12d8937e6447.png)
```
# docker의 volume 확인
docker volume ls
```
![화면 캡처 2022-07-12 201307](https://user-images.githubusercontent.com/87213815/178477315-329a83c6-3597-490b-85cf-399d383b2806.png)

### docker container 생성
```
# 사용할 포트 : 9000 
docker run -d --name portainer -p 9000:9000 --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:2.9.3
```
![화면 캡처 2022-07-12 201426](https://user-images.githubusercontent.com/87213815/178477535-a7102b77-77d3-4bbc-a4d8-3706794b1abf.png)
* -d : 백그라운드모드
* --name : 만드는 container 이름
* -p [호스트포트]:[이미지포트]
* --restart=always : 백그라운드에서도 실행
* -v [호스트폴더]:[container폴더] 
### 만들어진 192.168.56.1:9000으로 이동 후 ID,PW 등록 후 사용
![화면 캡처 2022-07-12 202047](https://user-images.githubusercontent.com/87213815/178478563-f3a3daf1-ee7c-4588-b77b-014c244396bc.png)

