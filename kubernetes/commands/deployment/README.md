# 📌 **nginx pod을 만들어서 k8s에 배포하기**

# Master에서 작업

```bash
# mynginx pod을 deployment 명령어로 만듬
k create deployment mynginx --image=nginx
```
![1](https://user-images.githubusercontent.com/87213815/183293578-a32562cd-bbf4-4beb-8d54-775c3d147b31.png)


✅ **확인방법**

- **k get deployments.apps**

  ![2](https://user-images.githubusercontent.com/87213815/183293589-4251836e-7d8e-4bb2-88dc-93b64b4a4c81.png)
   
- **k get pod -o wide**

  ![3](https://user-images.githubusercontent.com/87213815/183293599-571228a6-5f9b-4ef0-b627-89471d37e5cd.png)

- **k get all -o wide**
  ![4](https://user-images.githubusercontent.com/87213815/183293608-a20bb615-3aad-4077-8a25-0faa7a0526d6.png)
        
- **k get svc (k get service)**
    
    **→ 포트번호 확인가능**
    
- **k get rs**


## 🚨 **node2가 문제발생 !**

✅ master에서 curl 10.36.0.1 명령어로 **pod과 통신이 끊기는 것을 확인**

- 10.36.0.1 : weave network IP = pod IP

```
# 새로운 node로 실행되고 기존 node2 pod은 자동종료되는 것확인
k get pod -o wide
```
