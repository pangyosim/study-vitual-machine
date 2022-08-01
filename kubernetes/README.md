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
* * *

# Install kubernetes

## **master | node1 | node2 | node3 작업**

1. SWAP 기능 끄기
    
    ```bash
    swapoff -a
    vi /etc/fstab
    reboot
    ```
    
    <img src = "https://user-images.githubusercontent.com/87213815/182152100-958c0d1c-5170-4332-b1c0-a2926513f53e.png" width="800" height="200">
    
    ```bash
    modprobe br_netfilter
    echo '1' > /proc/sys/net/bridge/bridge-nf-call-iptables
    ```
    
    ```bash
    vi /etc/sysctl.conf
    
    # 내용추가
    ----------------------------------------
    net.bridge.bridge-nf-call-iptables = 1
    net.bridge.bridge-nf-call-ip6tables = 1
    ----------------------------------------
    reboot
    ```
    
    <img src = "https://user-images.githubusercontent.com/87213815/182152459-1922070e-34c6-4473-9212-fde28f357917.png" width="500" height="150">
    
    ```bash
    # kubernetes repository 다운
    cd /etc/yum.repos.d
    wget http://down.cloudshell.kr/k8s/kubernetes.repo
    ```
    
    ```
    vi /usr/lib/systemd/system/docker.service
    ExecStart=/usr/bin/dockerd 바로 뒤에 
    ----------------------------------------
    --exec-opt native.cgroupdriver=systemd
    ----------------------------------------
    를 삽입한다
    ``` 
    <img src = "https://user-images.githubusercontent.com/87213815/182152672-161b0c37-0363-4119-b6e1-a16db8c12314.png" width="500" height="300">
    
    ```
    systemctl daemon-reload
    systemctl restart docker
    
    # 확인
    docker info | grep -i cgroup
    ```
    
    <img src = "https://user-images.githubusercontent.com/87213815/182153051-cb986df6-e3d0-4031-94c5-1402f1aca632.png" width="800" height="80">
    
    ```
    #kubernetes 설치
    yum install -y kubelet-1.21.1-0 kubeadm-1.21.1-0 kubectl-1.21.1-0
    systemctl daemon-reload
    systemctl enable kubelet --now
    ```
    
    ## **Master에서 작업 (Clustering 구축)**
    
    ```
    #Clustering 구성하기 
    kubeadm init --kubernetes-version=v1.21.1
    
    # Master API로 commands를 config 
    mkdir -p $HOME/.kube
    cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    chown $(id -u):$(id -g) $HOME/.kube/config
    
    # 꼭 복사
    ⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐
    kubeadm join 10.0.2.12:6443 --token il46it.62rwg4lqjokztqbk \
            --discovery-token-ca-cert-hash sha256:92001b9269af854651db34a3e1f7e4fd19c06803c53717b7dbc3df4323d09f77
    ⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐
    ```
    
    <img src = "https://user-images.githubusercontent.com/87213815/182153293-2dcbc123-ad1d-4530-bb49-4c3a8ee3d552.png" width="600" height="300">
    
    ```
    # version 확인 (kubectl: 제어, kubeamd: 관리자, kubelet: )
    kubectl version --short
    kubeadm version -o yaml
    kubelet --version
    ```
    
    ```
    #weave-net 설치
    export kubever=$(kubectl version | base64 | tr -d '\n')
    kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$kubever"
    ```
    
    ![Untitled](https://user-images.githubusercontent.com/87213815/182153523-4c9727a4-9be8-46ef-8482-dc137ece9c73.png)

    ```
    kubectl get pods -A
    kubectl get pod -n kube-system
    ```
    
    <img src = "https://user-images.githubusercontent.com/87213815/182153615-29023137-188b-411b-b2c9-34544d532034.png" width="600" height="150">
    
    ```bash
    kubectl get nodes
    ```
    
    <img src = "https://user-images.githubusercontent.com/87213815/182153958-de074f38-597b-4a4f-b1c0-cbf1a4995629.png" width="500" height="50">
    
    # **node1 | node2 | node3 에서만 작업**
    
    ```bash
    # node1, node2, node3 실행
    kubeadm join 10.0.2.12:6443 --token il46it.62rwg4lqjokztqbk \
            --discovery-token-ca-cert-hash sha256:92001b9269af854651db34a3e1f7e4fd19c06803c53717b7dbc3df4323d09f77
    
    # master에서 확인
    kubectl get nodes
    ```
    
    <img src = "https://user-images.githubusercontent.com/87213815/182154116-4ad78a53-98eb-4300-97b4-1fd71db4c5b8.png" width="500" height="100">
    
    **Kubernetes command**
    - `kubeadm`: 클러스터를 부트스트랩하는 명령이다.
    - `kubelet`: 클러스터의 모든 머신에서 실행되는 파드와 컨테이너 시작과 같은 작업을 수행하는 컴포넌트이다.
    - `kubectl`: 클러스터와 통신하기 위한 커맨드 라인 유틸리티이다.






  
