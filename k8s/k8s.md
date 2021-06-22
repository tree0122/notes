## 第一节 IT技术发展趋势(k8s背景)

### docker相关概念
- 镜像：
    1. 分层构建, aufs, overlayfs2
    1. 镜像格式: repo:tag
- 容器
- Docker Host(docker daemon)
- docker registry
    1. Docker Hub, 
    1. Quey
- container, image, network, volume,...
- docker-compose

### docker 网络模型
- none
- bridge
- host
- overlay(vxlan, valn), 叠加网络
- docker环境跨宿主机通信
    - 各宿主机SNAT -> DNAT
    - 叠加网络(覆盖网络)，隧道网络：但是因mtu的降低，效率降低
    - 三层网络：每个宿主机有强大路由表，利用注册中心存储其他容器的下一跳路由
    - 问题：频繁动态调整容器，需要强一致性的注册中心

### 容器编排工具
- k8s
    1. CNCF组织: 谷歌为推广k8s而成立的
    1. Istio: 服务网格
- swarm
- mesos(DC OS)+marathon

### k8s
operator: 复杂
controller: 简单的控制器

master node: 管理节点
- Api Server
- Controller Mannager
- Scheduler
- Etcd

worker node: 工作节点
- kubelet: 与master的ApiServer联系
- kube-proxy: 代理服务组件
- container engine: 容器引擎

## 第二节 安装k8s

### 声明式配置
### 运行应用
- Controller
- Service

### 部署
1. 要求
    - 所有节点时钟同步
        
            yum -y chrony
            systemctl start chrony
            systemctl enable chrony
    - 防火墙关闭

            systemctl stop firewalld
            systemctl disable firewalld
            systemctl restart docker （可能需要）
    - 禁用swap
        
            swapoff -a (临时关闭)
            vi /etc/fastab: # *swap, (永久关闭)
    - 所有节点id唯一
    - vi /etc/sysctl.d/k8s.conf
        
            sysctl -p /etc/sysctl.d/k8s.conf
            sysctl -a 
    
    查看命令:
    - lscpu
    - free -m
    - 

1. 各类节点所需部署的组件

        master
            apserver, controller-manager, scheduler, etcd
        worker
            kubelet, kube-proxy, docker
        每个节点都需要
            docker, kubelet, kubeadm, kubectl
        所需命令:
            docker version
        注意： mirrors.aliyun.com

1. 利用repo, 

        安装 kubelet kubeadm kubectl
        cat repo // 
        setenforce 0
        yum install kubelet kubeadm kubectl

1. 初始化控制平面，主节点 kubeadm init(50m时刻)

        配置各节点域名 vi /etc/hosts

        kubeadm init --help

        kubeadm init --image-repository regitsty.aliyuncs.com/google_containers --kubernetes-version $Version
        --control-plane-endpoint $MASTER (控制平面的地址)
        --apiserver-advertise-address $MASTER_IP (apiserver的地址)
        --pod-network-cidr 10.244.0.0/16 
        --token-ttl 0 (token不过期)

        查看需要的镜像命令：kubeadm config images list

        cgroupfs警告：vi /etc/docker/daemon.json 
1. 普通用户，

    cp /etc/kubernets/config ~/.kube/config, 目的顺利执行 kubectl
1. 安装flannel(cri)

    kubectl apply -f $github.com/flannel.yaml
1. 查看安装情况

    kubectl get nodes/cs
    kubectl get pods -n kube-system
1. 如果发生错误，需重新部署

    kubeadm reset
1. vi kubeadm-init: 保存join的token
1. 在node节点

    kubeadm join $master:6443 --token $token --discovery-token-ca-cert-hash $hash
1. 运行容器

        kubectl create deployment demoapp --image="ikubernetes/demoapp:v1.0" --dry-run=client
        docker加速器地址(82min):
            ustc
            163 
            azk
            cn
            qiniu
1. 扩展容器 

        kubectl scale deployment demoapp --replicas=3  
        kubectl delete pods $podName

## 第三节

### 分布式Kube Cluster
- API Server
    - 访问入口：https，6443
    - 存储：etcd(raft协议， k/v存储)
- Controller Manager: 代码化的运维
    Controller(control loop)
- Scheduler: 调度Pod

### Node
- kubelet: agent
- Docker
- kube-proxy

### Add-ons
- KubeDNS: CoreDNS
- Dashbord: Web UI
- 监控系统: prometheus
- 集群日志系统: EFK, LG

    EFK
    - ElasticSearch
    - Filebeat/Fluentd/fluent-bit/logstash(JRuby语言实现)
    - Kibana

    LG, 是云原生应用
    - Loki
    - Grafana

- Ingress Controller: 控制外部流量进入k8s集群

### ca认证和peer2peer通信
![ca](../ref/img/ca.jpg)

### k8s集群部署和运行模型
1. 二进制程序
1. pod, kubelet管理这些Pod
    - Static Pod
    - Pod
1. kaas: 云服务商提供的部署

![k8s安装需要的pod](../ref/img/k8s-pod.jpg)

### k8s的分层 
- Addon Operator: Cluster Api, Addons
- kubeadm: Kubernetes Api, Bootstrapping
- Cluster Api: Machines, Infrastructure

#### k8s的分层图
![kubeadm-func](../ref/img/kubeadm-func.jpg)

#### kubeadm部署的pod
![kubeadm部署的pod](../ref/img/kubeadmPod.jpg)

### k8s容器间网络接口
- CNI: Container Network Interface
- k8s仅提供CNI，没有默认实现，具体实现有:
    - flannel(网络方案), Canal(网络策略)
    - Project Calico，网络+策略，
- CRI: Container Runtime Interface
- CSI: Container Storage Interface

![cni](../ref/img/cni.jpg)

#### k8s节点网络
- 节点网络
- Pod网络
- Service网络

![k8sNet](../ref/img/k8sNet.jpg)

kubeadm的执行流程
#### kubeadm的执行流程
![kubeadm的执行流程](../ref/img/kubeadmFlow.jpg)


### namespace资源


## 第四节(podAndResource)

### 容器关系
1. 亲密: 同Pod内 同进同出，共享一组存储卷、network、IPC、UTS
1. 非亲密:
    - Pod to Pod: 借助于的网络插件
    - Pod to Service: iptables, ipvs规则
        kube-proxy: 把集群上的每个service定义转换为本地的ipvs或者iptables规则
    - External Client -> Service or Pod

    Infrastructure container, 如Pause(暂停)，Pod相当于一主机()

### Deployment(Pod控制器基本介绍，40min)
1. template: Pod的定义

### apiServer的交互方式
1. 命令式命令: 命令，及选项来实现
1. 命令式配置文件: 命令，从配置文件中加载数据
1. 声明式配置文件: 声明式命令，从配置文件中加载数据

### 资源规范，配置清单
    ```
    apiVersion: ... # 资源对象所属的api群组及版本
    kind: ... # 资源类型
    metadata: # 资源对象的原数据
        ...
    spec: # 所需状态，或称为期望状态
        ...
    ```
kubectl explain $resource
kubectl api-resources

## 第五节(resource)

### 网络模型

1. 节点网络
1. Pod网路
1. 集群网路(服务网路)
各自独立的网段，相互间通信必须借助节点完成

网路模型上k8s内的四种常见通信
1. 同Pod内的容器间通信
1. Pod到Pod间通信
1. Pod到service的通信
1. 集群外部到service通信

![net](../ref/img/05net.jpg)

ResfulAPI, https
资源类型，schema
    Pod, Deployment, Service
对象，object

### 资源规范

    apiVersion, Kind, metadata, spec, status
    控制器：负责确保将实际状态设定不断逼近或者等同于期望状态
    Control Loop
        create, delete, change

### 资源分类：
1. 工作负载型(workload), Pod
    - 无状态(stateless)：Deplyment, DaemonSet(系统级应用)
    - 有状态(stateful)：StatefulSet -> Operator(：StatefulSet之上的专用应用)
    - 任务：Job, Cronjob
1. 服务发现和负载均衡
    - Service 
    - Ingress
1. 配置和存储(CSI, volume)
    - ConfigMap/Secret
    - PVC/PV
    - Downward API
    - role, rolebinding
1. 集群级别资源
    - namespace
    - node 
    - clusterrole
1. 元数据类型
    LimitRange, ...

### 资源查看(30min)
    ```
    kubectl api-resources 
    kubectl explain configMap

    ```
    kubectl get --raw /apis/GROUP/VERSION/namespaces/NAMESPACE/podes/POD

    apiServer就是一个http服务器

    kubectl get pods/$pod_name -o yaml

### 资源引用格式
    <type>/<name>: pods/mypod, deployment/demoapp
    <type> <name>: pods mypod, deployment demoapp

### 常用命令
1. kubectl get pods
1. kubectl apply -f ***.yaml
1. kubectl delete pods/pods_name    
1. kubectl logs $pods_name    
1. kubectl describe pods/$pods_name    
1. kubectl exec $pods_name -c $container_name -- cmd
1. kubectl edit  (实时编辑、修改，但资源的某些字段不运行实时修改)
1. kubectl apply -f RS.yaml: 新增+修改+删除

### namespace(集群级别资源)
1. kubectl get namespaces
1. kubectl get all
1. 在yaml中的资源分割符: --- (独占单行)


## 第六节(nodeAndPod) 

### json格式化(2min)
yum install epel-release
yum instal jq

### node资源(4min)
1. 查看node资源
    ```
    kubeclt get nodes
    kubeclt get nodes/$node_name -o wide
    ```
    
1. node-controller, 健康检查
    1.1.3之前: kubelet每10s给Apiserver发送一次node健康状态;连续4个周期未收到node状态，标识其为notReady。但每次带上images的信息比较大，浪费网络资源

    1.1.3之后: 节点租约，每10s在status发送变化或者超过5分钟后，上报一次到apiServer
    kubectl get leases -n kube-node-lease

    k8s上每个资源都有对应资源控制器，但资源控制器可能管理多种资源

### pod资源(28min)
#### 定义

    共享IPC UTS Network等名称空间的一组容器
    基础pod: pause的主要作用， user/mount

#### pod的常用相位: 
1. Running, 
1. Pending(未调度成功, 仍处于下载中), 
1. Succeeded(成功终止，如job资源)
1. Failed
1. Unkown: kubelet无法通信

#### Pod容器的常见状态：
1. CrashLoopBackOff, 
1. Error: pod中容器的状态

#### Pod中的多个容器(39min)
1. Sidercar: 为主容器提供辅助功能的，如：日志采集功能
1. Adapater: 使主容器其兼容k8s原生功能
1. Ambassador(大使): 方便主容器主动对外联络

    hook(钩子): post start, pre stop
    probe(探针): startup, liveness, readiness

#### 查看容器信息   
    kubectl describe pods/$POD_NAME

### Pod中的环境变量: 通过环境变量向容器传递信息

    env(容器内变量)
    云原生是支持环境变量的，非云原生不支持
    ```
    spec:
        containers:
        - name: demoapp
            image: ikubernetes/demoapp:v1.0
            env:
            - name: HOST
            value: "127.0.0.1"
            - name: PORT
            value: "8080"
    ```
    
#### Pod暴露端口，通过hostPort暴露容器服务
```
kind: Pod
metadata:
  name: mypod-with-ports
  labels:
    app: mypod
    release: canary
spec:
  containers:
  - name: demoapp
    image: ikubernetes/demoapp:v1.0
  - ports:
    - containerPort: 80
      name: http
      protocol: TCP
      hostPort: 10080
```

### pod级别的hostNetwork，共享宿主机的网络名称空间，但是很不安全(65min)
```
kind: Pod
metadata:
  name: mypod-with-ports
  labels:
    app: mypod
    release: canary
spec:
  containers:
  - name: demoapp
    image: ikubernetes/demoapp:v1.0
  - ports:
    - containerPort: 80
      name: http
      protocol: TCP
  hostNetwork: true
```

作业: wordpress, mysql


***
## 第七节(securityContext) 

### 容器运行时CRI的实现
- podman
- docker

[PodStruct](../ref/img/07PodStruct.jpg)

### 资源规范格式(15min)

```
// kubectl explain (查看字段含义)
apiVersion: Group/Version
kind: 
metadata:
    name: ...
    namespace: ...
spec:
```
apiVersion规范 // kubectl api-versions: 查看所有版本
    - alpha(内测版): vialpha1, v1alpha2
    - beta: v1beata1
    - v1

### 容器的安全上下文，Security Context(25min)
pod的隔离集群
1. 定义：一组用于决定容器如何创建和运行的约定条件，他们代表创建和运行容器时使用的运行时参数
1. Pod上SC有两个级别
    - Pod级别：对pod中的所有容器生效
    - 容器级别：只对当个容器生效
    - PSP级别：PodSecurityPolicy

```
apiVersion: v1
kind: Pod
metadata: {}
spec: 
  securityContext: # Pod级别的安全上下文，对内部所有容器生效
    runAsUser <integer> # 以指定用户身份运行容器，默认由镜像中的USER指定
    runAsGroup <integer> # 以指定用户组运行容器，默认使用的组随容器运行时
    supplementalGroups <[]integer> # 为容器中1号进程的用户添加的附加组
    fsGroup <integer> # 为容器中1号进程附加的一个专用组，其功能类似sgid
    runAsNonRoot <boolean> # 是否以非root身份运行
    seLinuxOptions <object> # SELinux的相关配置
    sysctls <[]object> # 应用到当前Pod上的名称空间级别的sysctl参数设置列表
    windowsOptions <object> # windows容器专用的设置
  containers:
  - name: ..
    image: ..
    securityContext: # 容器级别的安全上下文，对当前容器生效
      runAsUser <integer> # 以指定用户身份运行容器，默认由镜像中的USER指定
      runAsGroup <integer> # 以指定用户组运行容器，默认使用的组随容器运行时
      runAsNonRoot <boolean> # 是否以非root身份运行
      allowPrivilegeEscalation <boolean> # 是否允许特权升级
      capabilities <object> # 于当前容器上添加（add）或删除（drop）的内核能力
        add <[]string> # 添加由列表定义的各内核能力
        drop <[]string> # 移除由列表定义的各内核能力
      privileged <boolean> # 是否运行为特权容器
      procMount <string> # 设置容器的procMount类型，默认为DefaultProcMount;
      readOnlyRootFilesystem <boolean> # 是否将根文件系统设置为只读模式
      seLinuxOptions <object> # SELinux的相关配置
      sysctls <[]object> # 应用到当前Pod上的名称空间级别的sysctl参数设置列表
      windowsOptions <object> # windows容器专用的设置
```

常见的capabilities：
- CAP_CHOWN: 改变uid和gid
- CAP_MKNOD: 创建设备文件
- CAP_NET_ADMIN: 网络管理权限
- CAP_SYS_ADMIN: 大部分管理权限
- CAP_SYS_TIME: 修改系统时间
- CAP_SYS_MODULE: 装卸载内核模块
- CAP_NET_BIND_SERVER: 是否允许普通用户绑定1024内端口

imagePullPolicy(镜像拉取策略)
    Never: 从不拉取
    IfNotPresent: 不存在才去拉取；非latest标签的默认值
    Always: 总是拉取；latest标签的默认值

setcap / getcap：命令直接设定权限

command: 自定义image要运行的程序
args: 向自定义的command传递参数

kube-proxy功能: 是将apiServer中的service变动，同步到对应节点的iptables和ipvs变动

Pod内可设定的安全参数：
- kernel.shm_rmid_forced, 
- net.ipv4.ip_local_port_range, 
- net.ipv4.tcp_syncookies
修改如： /etc/default/kubelet
KUBELET_EXTRA_ARGS='--allowed-unsafe-systcls=net.core.somaxconn,net.ipv4.ip_unprivileged_port_start'


***

## 第八节(probeAndHook)

![08ProbeHook.jpg](../ref/img/08ProbeHook.jpg)
 
### Cloud Native中的服务所需的基础功能
1. process health: 进程健康
1. metrics: 指标
1. readiness: 准备
1. liveiness: 存活
1. tracing: 追踪
1. logs: 日志 

### 容器探针（10min）

```
# Pod的yaml
spec:
  containers:
  - name: ...
    image: ...
    livenessProbe:
      exec <object>                 # 命令式类型探针
      httpGet <object>              # http GET类型探针
      tcpSocket <object>            # tcp Socket类型探针
      initialDelaySeconds <integer> # 发起初次探测请求的延后时长
      periodSeconds <integer>       # 请求周期
      timeoutSeconds <integer>      # 超时时长
      successThreshold <integer>    # 成功阈值
      failureThreshold <integer>    # 失败阈值
```

三种探针场景：
1. LivenessProbe: 周期性检测，检测未通过，kubelet会根据restartPolicy的定义决定是否重启该容器; 未定义时，kubelet认为容器未终止，即为健康

1. ReadinessProbe: 周期性检测，检测未通过，与Pod关联的Service，会将此Pod从service的后端可用列表删除; 直到再次就绪，重新添加回来；未定义时，只要容器未终止，即为就绪；

1. StartupProbe: 便于用户使用同LivenessProbe的不同参数或阀指；

三种探针方式：
1. ExecAction：直接执行命令，命令成功返回即成功
1. TCPSocketAction：端口能正常打开，即成功
1. HTTPGetAction：向指定path发送http请求，2xx 3xx响应表示成功

### 容器生命周期的钩子(48min)

![08ContainerLifeCircle.jpg](../ref/img/08ContainerLifeCircle.jpg)

```
spec:
  containers:
  - name: demo
    image: ikubernetes/demoapp:v1.0
    imagePullPolicy: IfNotPresent
    securityContext:
      capabilities:
        add:
        - NET_ADMIN
    livenessProbe:
      httpGet:
        path: '/livez'
        port: 80
        scheme: HTTP
      initialDelaySeconds: 5
    lifecycle:
      postStart:
        exec:
          command: ['/bin/sh', '-c', 'iptables -t nat -A PREROUTING -p tcp --dport 8080 -j REDIRECT --to-ports 80']
      preStop:
        exec:
          command: ['/bin/sh', '-c', 'while killall python3; do sleep 1; done']
```

### Pod中的多容器（55min）
1. Sidecar: 为主容器提供辅助功能(外部进入容器)，如代理等
1. Adapter: 使主容器能适配外部环境, 如nginx的日志输出适配于promehteus
1. Ambassador: 使主容器更好的接入外面多变环境(容器内部输出于外部)
1. initContainers: 
    - 先启动成功后，再启动主容器
    - 初始化容器执行完，即退出

```
// initContainer
spec: 
  initContainers:
  - name: iptables-init
    image: ikubernetes/admin-box:lastest
    imagePullPolicy: IfNotPresent
    command: ['/bin/sh', '-c']
    args: ['iptables -t nat -A PREROUTING -p tcp --dport 8080 -j REDIRECT --to-ports 80']
    securityContext:
      capabilities:
        add:
        - NET_ADMIN
  containers:
  - name: demo
    image: ikubernetes/demoapp:v1.0
    imagePullPolicy: IfNotPresent
    ports:
    - name: http
      containerPort: 80

// sidecar
spec: 
  containers:
  - name: proxy
    image: envoyproxy/envoy-alpine:v1.14.1
    command: ['/bin/bash', '-c']
    args: ['sleep 5 && envoy -c /etc/envoy/envoy.yaml']
    lifecycle:
      postStart:
        exec:
          command: ['/bin/sh', '-c', 'wget -O /etc/envoy/envoy.yaml http://ilinux.io/envoy.yaml']
  - name: demo
    image: ikubernetes/demoapp:v1.0
    imagePullPolicy: IfNotPresent
    env:
      name: HOST
      value: "127.0.0.1"
      name: PORT
      value: "8080"
```



## 第九节(computeResource)
### k8s计算资源规范 compute resource, reqeust, limit

1. request（下阈值）: 确保节点至少为Pod或者容器配备的资源最小量
1. limit（上阈值）: 确保节点至少为Pod或者容器配备的资源最大量
1. k8s计算资源主要指：
    - CPU，可压缩型；CPU单位：多个CPU核心，1核=1000m核
    - Memory，不可压缩型；单位：1Ki, 1Mi, 1Gi
    - Volume，

```
spec: 
  containers:
  - name: demo
    image: ikubernetes/stress-ng:v1.0
    imagePullPolicy: IfNotPresent
    command: ['/usr/bin/stress-ng', '-c', '']
    resources:
      requests:
        cpu: '200m'
        memory: '128Mi'
      limits:
        cpu: '500m'
        memory: '512Mi'
```

### Pod的服务质量(QoS Class)，代表Pod的资源被优先满足的类别（45min）
1. Guaranteed: 必须，Pod内的每个容器分别限制计算资源，且上下阈值相同
1. Bustable: 中间，
1. BestEffort: 尽力，未对任何容器设定任何需求或限制

***

## 第10节(serviceBase)

![PodRelations](../ref/img/10PodRelations.jpg)

### 定义
    Service：标准的资源类型，Service Controller
        为动态的一组Pod提供一个固定的访问入口，ClusterIP(Cluster Network)
    Endpoint(IP:PORT)：标准资源类型，Endpoint controller, 通过标签选择器监控和管理pod和service的映射; Service借助其实现其功能

    kube-proxy, Service Controller位于各节点上agent：

### service代理模型：kube-proxy如何确保service能正常工作
1. userspace模型：Pod -> Service, iptables拦截规则，但不调度，将流量转给kube-proxy(此时类型nignx的反响代理)
1. iptables模式，一个service会生成大量的规则
1. ipvs模式

![kubeProxyAndService](../ref/img/10kubeProxyAndService.jpg)

### Service类型：
1. ClusterIP：通过集群内部IP地址暴露服务，但是该地址仅在集群内部可达，无法被集群外客户端访问；默认类型
1. NodePort：是ClusterIP的增强类型，在ClusterIP的功能之上，在每个节点上使用一相同端口号，将外部流量引入该service
1. LoadBalancer：是NodePort的增强型，要借助于底层IaaS云服务的LBaaS按需管理LB
1. ExternalName：借助集群上的kubeDNS实现，服务的名称会被解析为一个CNAME记录，而CNAME名称会被DNS解析为集群外部的服务的IP地址；没有ClusterIP和NodePort


## 第11节(service)

```
    kind: Service
    metadata:
        name: -
        namespace: -
    spec:
        type<string> # 默认ClusterIP
        selector <map[string]stirng> # 等值
        ports:
        -   name: 
            protocol:
            port:
            targetPort:
            nodePort:
```

### labelSelect, label

### 管理集群外部的服务(71min)
1. 认为创建Endpoint，关联外部服务
1. 认为创建Service，关联对应Endpoint


***
## 第12节(kubeProxyAndDns)

### iptables代理模式下的ClusterIP，每个service在每个节点上，都会生成相应的iptables规则
1. KUBE-SERVICES: 包含所有ClusterIP类型的Service的流量匹配规则，有PREROUTING和OUTPUT两个内置链直接调用；每个Service对象包含两条规则定义，对于所有发往该Service(目标ip为Service_IP且目标端口为Service_Port)的请求报文，前一条用于为那些非源自Pod网络(! -s 10.244.0.0/16)中请求报文借助于KUBE-MARQ-MASK自定义链中的规则打上特有的防火墙标记，后一条负责将所有报文转至专用的以KUBE-SVC为名称前缀的自定义链，后缀是Service信息的hash值

1. KUBE-NODEPORTS

1. KUBE-MARK-MASQ: 专用目的自定义链，所有转至该自定义链的报文都将被置入特有的防火墙标记(0x4000)，以便于将特定的类型的报文定义为单独的分类，目的在将该类报文转发到目标端点之前，由POSTROUTING规则链进行源地址转换

1. KUBE-SVC-<HASH>: 定义一个服务的流量调度规则，它通过随机调度算法(random)将请求发送给该Service的所有后端端点，每个后端端点定义在以KUBE-SEP为前缀名称的自定义链上，后缀是端点信息的hash值

1. KUBE-SEP-<HASH>: 定义一个端点相关的流量处理规则，它通常包含两条规则，前一条用于为那些源自该端点自身(-s ep_ip)的请求流量调用自定义链KUBE-MARQ-MASK打上特有防火墙标记，后一条负责将发往该端点的所有流量进行目标IP地址和端口转换，新目标为该端点的IP和端口(-j DNAT --to-destination ep_ip:ep_port)

1. KUBE-POSTROUTING: 专用的自定义链，有内置链POSTROUTING无条件调用，负责将拥有特有防火墙标记0x4000的请求报文进行源地址转换(target为实现地址伪装的MASQUERADE)，新的源地址为报文离开协议栈时流经接口的主IP(primary ip)地址


### ipvs代理模式(10min)
kube-proxy会在每个节点上创建名为kube-ipvs0的虚拟接口，并将集群所有service对象的ClusterIP和ExternalIP都配置在该接口；kube-proxy为每个service生成一个虚拟服务器(virtual server)的定义
1. nat, 仅需要借助少量iptables规则，完成源地址转换等功能
1. 修改集群为ipvs模式

### service的注册和发现：
1. zk, Euraka, Consul...
1. 将传统DNS服务直接提供一个云原生解决方案，他支持从apiServer动态加载相关的service及端点信息，并自动生成资源记录
1. 服务注册和发现的总线: kubeDNS
    - SkyDNS
    - KubeDNS
    - CoreDNS

docker的服务发现
1. 基于环境变量
1. 基于dns


## 第13节(headlessService)

headless service: 无头服务
stateful: 每个个体具有一定程度的独特性，由其存储的状态决定； 
stateless

## 第14节(volume)
docker的文件系统，与docker容器有同样的生命周期
外部存储空间：
    Host
    network storage

存储卷类型:
1. Host级别: hostPath, Local
1. 网路级别: nfs, GlusterFS, rbd(块设备), CephFS(文件系统)...
1. 临时存储: emptyDir

### Pod使用volume步骤
1. Pod上定义存储卷，并关联至目标存储服务上
1. 在需要存储卷的容器上，挂着其pod的存储卷


### pv/pvc(86min)
storageClass
1. pvc: persistent volume claim, 持久卷申请; k8s上标准的资源类型之一；由apiserver用户使用
1. pv: persistent volume, 持久卷，可被pvc绑定；而pv一定要关联与某个真正的存储空间(一般是网路存储服务上的存储空间)，由集群管理员管理
1. pvc创建后，需找到最为匹配的pv，并与之绑定

Pod使用这类存储的步骤：
1. admin: 创建pv
1. user: 按需创建pvc，而后创建Pod，在pod调用pvc类型的存储插件，调用同一个名称空间的pvc资源


## 第15节(29min)

### StorageClass: 模版 (43min)
1. pv和pvc都可能属于某个特定的sc
1. 模拟名称空间: 
1. 创建pv的模版: 将某个存储服务与sc关联起来

### kube-controller-manager 兼容ceph (56min)

***
## 第16节(configMap)
核心资源类型存储卷，PV, PVC, SC, CSI(Longhorn)
kubelet In-Tree
  插件: configMap, Secret, downwardAPI

如何为容器化应用提供配置信息：
1. 启动容器时，直接向应用程序传递参数，args: []
1. 将定义好的配置文件培进镜像中
1. 通过环境变量向容器传递配置数据：
1. 基于存储卷向容器传递配置文件：运行中的改变，需要应用程序重载

ConfigMap，以k/v格式保存配置项的名称和配置数据

### 创建方式
1. 命令式命令: kubectl create configmap demoapp-config --from-literal=host=0.0.0.0 --from-literal=port=8080
1. 命令行(文件): kubectl create configmap nginx-confs --from-file=./myserver.conf --from-file=status.cfg=./myserver-status.cfg
1. 命令行(目录): kubectl create configmap nginx-config-files --from-file=./nginx.conf.d/
1. 资源清单: 

### 使用configmap 
1. 基于键值的方式引用
  ```
  spec:
  containers:
  - image: ikubernetes/demoapp:v1.0
    name: demoapp
    env:
    - name: PORT
      valueFrom:
        configMapKeyRef:
          name: demoapp-config
          key: demoapp.port
          optional: false
  ```
1. 基于存储卷的方式引用
  ```
  spec:
  containers:
  - image: nginx:alpine
    name: nginx-server
    volumeMounts:
    - name: ngxconfs
      mountPath: /etc/nginx/conf.d/
      readOnly: true
    volumes:
    - name: ngxconfs
      configMap:
        name: nginx-config-files
        optional: false
  ```
  kubectl get pods/configmaps-env-demo -o go-template={{.status.podIP}}

### 修改configMap中的内容
    
    kubectl edit cm $cm_name: edit命令是立即生效的
    configMap类似配置中心的服务
    nginx -s reload
    nginx -T

  ***


## 第17节(secretDownAPI)
kubectl create secret --help
### secret类型
1. docker-registry: 创建一个给 Docker registry 使用的 secret
1. tls: 专门用于保存tls/ssl用于证书和配对的密钥
1. generic: 其他的类型
  - --type="kubernetes.io/basic-auth"
  - --type="kubernetes.io/rbd"
  - --type="kubernetes.io/ssh-auth"
  - kubeadm的bootstrap所使用的token

  另外，保存专用于ServiceAccount相关的token信息的secret资源，会使用资源注解来保存其使用场景
### annotations
1. annotation的名称遵循类似于labels的名称格式，但其数据长度不受限制
1. 不能用于被标签选择器作为筛选条件；但
1. 专用的管理命令: kubectl annotate type/name key=val

### tls
在创建secret的命令中，除了类型标识不同外，他还需要使用专用的选项，--cert=  和 --key=

无论证书和私钥文件名是什么，他们统一为tls.key和 tls.crt

### docker-registry
```
ubectl create secret docker-registry my-secret --docker-server=DOCKER_REGISTRY_SERVER --docker-username=DOCKER_USER --docker-password=DOCKER_PASSWORD --docker-email=DOCKER_EMAIL
```
1. 如何使用: pod.spec.imagePullSecrets

### secret的使用
1. pod的env(环境变量)的传值方式, 仅pod启动是读取一次env变量
  ```
  env:
  - name:
    valueFrom:
      secretKeyRef:
        name: mysql-root-auth
        key: password
  ```
1. volume(存储卷)的格式
  ```

  ```

### downwardAPI存储卷类型，
容器运行时，获取容器定义时的信息。严格意义上不是存储卷，原因在于，它引用的是Pod自身的运行环境信息，这些信息在Pod启动时就存在
1. fieldRef: metadata.name
1. status.
1. spec.
1. resourceFieldRef: 


***
## 第18节(ReplicaSet)
k8s以应用(Pod)为中心
应用编排
  部署、扩展、更新、回滚

### 负责应用编排的控制器有如下几种
1. ReplicationController: 早期的Pod控制器(已废弃)
1. ReplicaSet: 副本集, 负责管理一个应用的多个副本
1. Deployment: 部署(高级控制器)，不直接管理Pod，借助于ReplicaSet来管理Pod
1. DaemonSet: 守护进程集, 用于确保在每个节点上仅运行某个应用的一个Pod副本
1. StatefulSet: 功能类似Deployment，但专用于编排有状态应用
1. Job：有终止期限的作业式任务，而非一直处于运行状态的服务进程
1. CronJob：有终止期限的周期性作业任务

### 定义要素
1. 标签选择器
1. 期望的副本数
1. Pod模版
```
  kind: ReplicaSet
  spec:
    minReadySeconds <int> #
    replicas <int> #
    selector:
      matchExpressions <[]Object>
      matchLabels: <map[string][string]>
        app: demoapp
    template:
      metadata:
        labels:
          app: demoapp
      spec:
        containers:
```

### ReplicaSet的更新机制(手动删除式更新)
1. 单批次删除所有Pod，一次完成所有更新，服务会中断一段时间
1. 灰度(滚动)更新: 分批次更新
1. set image: 更新应用版本，但对replicaSet来说，仅能更新APIServer中的定义


***
## 第19节(blueGreenAndRoll)


***
## 第20节(Deployment&DaemonSet&Job&CronJob)
### Deployment
```
apiVersion: apps/v1
kind: Deployment
spec:
  minReadySeconds <int>
  replicas <int>
  selector <obj>
  tmeplate <obj> # Pod定义
  revisionHistoryLimit <int>
  strategy #更新策略
    type
    rollingUpdate
      maxSurge
      maxUnavailable
  progressDeadlineSeconds <int>
  paused
```
kubectl rollout --help # 滚动命令

### DaemonSet
```
apiVersion: apps/v1
kind: DaemonSet
spec:
  minReadySeconds <int>
  selector <obj>
  tmeplate <obj> # Pod定义
  revisionHistoryLimit <int>
  updateStrategy #更新策略
    type
    rollingUpdate
      maxUnavailable
```

### Job(69min)
```
apiVersion: batch/v1
kind: Job
spec:
  selector <obj>
  tmeplate <obj> # Pod定义
  completions <int>
  ttlSecondsAfterFinished <int>
  parallelism <int>
  backoffLimit <int>
  activeDeadlineSeconds <int>
```

### CronJob(80min)
```
apiVersion: batch/v1beta1
kind: CronJob
spec:
  jobTemplate <obj> # job作业模版
    metadata <obj>
    spec <obj>
  schedule <str>
  concurrencyPolicy <str> # 并发策略，Allow, Forbid, Replace
  failedJobsHistoryLimit <int>
  successfulJobsHistoryLimit <int>
  startingDeadLineSeconds <int>
  suspend <boolean>
```

***
## 第21节(StatefulSet)
### StatefulSet(20m)
通用的有状态应用控制器

1. 每个Pod都有自己的唯一标识，故障时，它只能被拥有同一标识的新实例所取代；
    - ¥{STATEFULSET_NAME}-${ORDINAL}: web-0, web-1, web-2
    - Headless Service
1. 若有必要，可为每个Pod配置专用的volume，其只能时PVC格式；

### awesome operator(有状态应用的集群管理器 35m)

### StatefulSet 格式
```
spec:
  replicas 
  selector
  template
  serviceName
  volumeClainTemplates
    apiVersion
    kind
    meatdata
    spec
  podMangementPolicy <str> # Pod管理策略，默认的"OrderedReady"表示顺利创建并逆序删除，另一可用值"Parallel"表示并行模式
```

### longhorn(50m)



***
## 第31节(Ingress)
### 外部流量进入集群内部的实现
1. Service:
  NodePort
  externalIp
1. host:
  hostPort
  hostNetwork
1. Ingress: 集群外部注入集群内部的流量
  Egress: 集群内部流出到集群外部的流量

### Ingress 和 Ingress Controller
Ingress: 是一个标准的资源
Ingress Controller: Ingress控制器
  http/https代理服务
    Ingress-Nginx: Kong
    HAProxy
    Envoy: Contour, Gloo
    Traefik

### Ingress规范(34m)
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name:
  annotations:
    kubernetes.io/ingress.class: <str>
spec:
  rules <[]obj>
  - host <str>
    paths <[]obj>
    - path
      pathType
      backend
        resouce
        service
          name:
          port:
            name:
            number:
  tls
  - hosts <[]str>
    secretName
  backend

```


***
## 第32节(Contour)
### Contour(38m)


***
## 第33节(kustomize)


***
## 第34节(helm)

