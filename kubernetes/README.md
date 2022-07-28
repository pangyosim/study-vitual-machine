# <img src = "https://user-images.githubusercontent.com/87213815/181480904-eab16fb3-0fc9-4367-9914-2c64adc878c6.png" width="35" height="35"> Kubernetes

### 💡 **서비스 관리를 개별적 → 집단적**

- 대부분 **개별 시스템(node)와 상호작용할 필요는 없다.**
- 비교적 Stateless(상태 비저장)
    
    Stateful : 관계상태를 유지 → 비교적 속도가 느림
    
    Stateless : 관계상태를 유지 하지 않음 → 속도가 좀 더 빠름 (default)
    
    - ex) 4계층 프로토콜 : TCP(stateful), UDP(stateless)
        
              7계층 프로토콜 : HTTP(stateless)

* https://kubernetes.io
  ## Docker container 강점
  * Application을 운영할 때 VM보다 **가볍다.**
    → OS를 안가지고있으며, **kernel을 공유한다.**
  * 배포가 매우 편리하며, 지속적인 개발과 통합에 유용하다.
    → **Scale-in/out이 쉽다.**
  * Resource를 충분하게 활용할 수 있다.
  
  ## Docker container 약점
  * 개발자 입장
    → container가 많을 수록 관리하기가 어렵다.
  * 운영자 입장
    → container가 갑자기 죽으면 살리기가 어렵다.
    → container가 실행중이면 image update를 못한다.
    → 서비스 이상 및 이슈의 모니터링이 어렵다.

### 이러한 약접을 보완한 것이 k8s - Container Orchestration

### 📌 K8S (container orchestration)

- 복잡한 container 환경을 kubernetes가 관리
- **Clustering** → 여러 node의 중앙관리 **(하나의 논리적인 단위로)**
- **상태(State)관리** → **Pod 개수 보장 (Pod livenessProbe) = 서버가 죽어도 자동으로 보정**
    - Pod livenessProbe : 하단에 있는 container들 또한 체크
    - Pod이 container을 감싸고있음.
- **부하(Scheduling)으로 배포 관리**
    
    → Pod 배포시 부하 정도가 **가장 적절한 node를 선정**하여 자동배포
    
- **배포(Deployment)** 버전관리
    - Deployment → pod [deployment가 pod보다 먼저 만들어지는 이유] : pod을 관리하기 위해, pod으로 pod을 만들면 죽어도 안살아나지만 deployment로 만들면 죽어도 **자동으로** 살아난다.
    - Update 문제시 **Rollback** 가능
    
- **service discovery**
    - Pod 집합에서 실행중인 **애플리케이션을 네트워크 서비스로 노출하여 Cluster 외부에 있는 user가 application을 이용**하도록
- **Volume Storage**
    - container 단위가 아니라 **Pod 단위**    
    
- **Container Orchestration**
    
    → 다수의 Container를 다수의 **node(Cluster)에 적절하게 분산 실행**하고, **원하는 상태로 실행상태를 유지해주고**, 다운타임 없이 **유동적으로 Scale을 확장/축소할 수 있게 도와준다.**
    
    → **배포(Deployment)/운영(Operation)/스케일링(Scaling)**
    
  
