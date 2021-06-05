### container

### network model
none, bridge, host

overlay(vxlan, vlan)

编排工具: k8s, swarm, mesos(DC OS)+marathon
Borg, kubernets, k8s, SDN
CNCF(组织), Istio(服务网格)

### k8s
operator
controller

#### master node: 控制平面
- API Server: 通信api
- Controller Manager
- Scheduler
- Etcd: 状态存储 

#### work node: 数据平面
- kubelet
- kubeproxy
- container engine

kubectl: 客户端工具

#### k8s运行
声明式配置
- Controller
- Service

#### k8s部署要求
- 时钟同步
- 防火墙关闭
- 禁用swapoff -a, 永久禁用 /etc/fstab
- id唯一
- getenforce 关闭

#### k8s部署组件
1. master
    kube-apiserver, kube-contoller-manager, kube-scheduler, etcd
1. worker
    kubelet, kube-proxy, docker
1. 要点，每个节点要有docker, kubelet, kubeadm, kubectl


yum install -y chrony
systemctl start chrony
systemctl enable chrony

 systemctl stop firewalld
 systemctl disable firewalld

 swapoff -a, 永久禁用 /etc/fstab 的swap

 cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sudo sysctl --system

## cgroupdriver
cat <<EOF> /etc/docker/daemon.json 
 {
   "exec-opts": ["native.cgroupdriver=systemd"]
}
EOF


 kubeadm init --image-repository registry.aliyuncs.com/google_containers --kubernetes-version v1.21.1 --control-plane-endpoint master01.tr --apiserver-advertise-address 10.1.1.155 --pod-network-cidr=10.244.0.0/16 


To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

 export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/


You can now join any number of control-plane nodes by copying certificate authorities
and service account keys on each node and then running the following as root:

  kubeadm join master01.tr:6443 --token qc1ayl.2o6oaa89ugy5388l \
	--discovery-token-ca-cert-hash sha256:0f4ab7f5ba9c2e44babf08213ac657cba578adedeac0f87a0cb43bf7ede0808a \
	--control-plane 

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join master01.tr:6443 --token qc1ayl.2o6oaa89ugy5388l \
	--discovery-token-ca-cert-hash sha256:0f4ab7f5ba9c2e44babf08213ac657cba578adedeac0f87a0cb43bf7ede0808a 



### ubuntu computer

 kubeadm join k8s.tr:6443 --token k7locd.daf4iw8wgzom8no5 \
    --discovery-token-ca-cert-hash sha256:83f9e705795744e43d7b859b237730b3c9322785876b9605001a742fbebb132d \
    --control-plane 

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join k8s.tr:6443 --token k7locd.daf4iw8wgzom8no5 \
    --discovery-token-ca-cert-hash sha256:83f9e705795744e43d7b859b237730b3c9322785876b9605001a742fbebb132d 


 ## 06视频
 ### k8s的用户
 - service cleint
 - developer operator

### 资源
资源类型, schema
    pod, deployment, service
对象(实例化后的), object
资源规范
    apiVersion, kind, spec(期望状态), status(实际状态)
    控制器：负责确保实际状态不断逼近或等同于期望状态
        create, delete, change
资源分类：
工作负载型(workload), Pod
    stateful, stateless
    无状态：Deplyment, DaemonSet(系统级应用)
    有状态：StatefulSet -> Operator(ss之上的应用)
    任务：Job, Cronjob
服务发现和负载均衡
    Service, Ingress
配置和存储
    ConfigMap/Secret
    PVC/PV
    Downward API
    role, rolebinding
k8s内作用域
    namespace
集群级别资源
    namespace, node, clusterrole
元数据类型
    LimitRange,...
资源路径
    /apis/GROUP/VERSION/namespaces/NAMESPACE/podes/POD

kubectl proxy: 可实现 curl

kubectl explain pods.metadata: 查看资源说明

资源引用格式
    <type>/<name>: pods/mypod, deployment/demoapp
    <type> <name>: pods mypod;仅同类型的多个删除

kubectl get NODE
kubectl create -f POD.yaml
kubectl delete POD
kubectl log POD_NAME
kubectl describe pods POD_NAME
kubectl exec 
kubectl edit  
kubectl apply -f RS.yaml: 新增+修改

单个资源文件配置多个资源清单

