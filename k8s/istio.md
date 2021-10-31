## 01outline

1. Service Mesh Basics
1. Envoy Basics
1. 基础配置及应用
1. xDS协议及API动态配置
1. 层级化运行时
1. 服务韧性
1. 流量治理
1. 分布式追踪
1. Istio基础
1. Istio实践


--- 

## 02servcieMesh

### 微服务治理面临的挑战
![02microSrvQues](../ref/img/istio/02microSrvQues.jpg)

### 服务网格
- 概念：专注于处理服务间通信的基础设施，它负责在现代云原生应用组成的复杂拓扑中可靠的传递请求
- 治理模式：除了处理业务逻辑的相关功能外，还包括此前单体应用中的网络通信、熔断、限流、跟踪等
- 实现模式
    - 内嵌于应用程序
    - SDK
    - Sidecar

![02serviceMeshSideCar](../ref/img/istio/02serviceMeshSideCar.jpg)


![02controlPlaneDataPlane](../ref/img/istio/02controlPlaneDataPlane.jpg)


![02meshFunc](../ref/img/istio/02meshFunc.jpg)


![02meshImpl](../ref/img/istio/02meshImpl.jpg)


![02meshDeploy](../ref/img/istio/02meshDeploy.jpg)


---

## 03EnvoyArch

### 什么是Envoy

Envoy是一个L7代理和连接总线，用于现代大型面向服务架构

![03whatEnvoy](../ref/img/istio/03whatEnvoy.jpg)

### Envoy组件拓扑

![03EnvoyComponent](../ref/img/istio/03EnvoyComponent.jpg)

![03EnvoyComp](../ref/img/istio/03EnvoyComp.jpg)

![03EnvoyTerm](../ref/img/istio/03EnvoyTerm.jpg)

![03EnvoyImpl](../ref/img/istio/03EnvoyImpl.jpg)

![03EnvoyChar](../ref/img/istio/03EnvoyChar.jpg)

### Envoy xDS核心术语

![03xDS_API](../ref/img/istio/03xDS_API.jpg)

### Envoy部署类型

### Envoy核心配置组件

### Envoy线程模型和连接处理机制


---

## 04EnvoyTheory

![04DeployType](../ref/img/istio/04DeployType.jpg)

![04ThreadModel](../ref/img/istio/04ThreadModel.jpg)

![04Connection](../ref/img/istio/04Connection.jpg)


---

## 05xDS

![05xDS_conf](../ref/img/istio/05xDS_conf.jpg)

![05concept](../ref/img/istio/05concept.jpg)

![05ConfIntro](../ref/img/istio/05ConfIntro.jpg)

![05listenerCluster](../ref/img/istio/05listenerCluster.jpg)

![05listener](../ref/img/istio/05listener.jpg)

![05L4filter](../ref/img/istio/05L4filter.jpg)

![05L4filter_1](../ref/img/istio/05L4filter_1.jpg)

![05L4fitler_2](../ref/img/istio/05L4fitler_2.jpg)

![05cluster](../ref/img/istio/05cluster.jpg)


---

## 06EnvoyInit的入门使用

![06startups](../ref/img/istio/06startups.jpg)

### Envoy安装概况
![06install](../ref/img/istio/06install.jpg)

### Building Envoy with Bazel
![06buildBazel](../ref/img/istio/06buildBazel.jpg)

![06buildBazel_1](../ref/img/istio/06buildBazel_1.jpg)

![06buildDocker](../ref/img/istio/06buildDocker.jpg)

![06confGen](../ref/img/istio/06confGen.jpg)

![06baseImage](../ref/img/istio/06baseImage.jpg)

![06toolApp](../ref/img/istio/06toolApp.jpg)

![06startEnvoy](../ref/img/istio/06startEnvoy.jpg)

![06listenerConf](../ref/img/istio/06listenerConf.jpg)

### Envoy的第一个实例(50m)
![06instance](../ref/img/istio/06instance.jpg)

 
---

## EnvoyLayer4

### Cluster简易静态配置
![07ClusterConf](../ref/img/istio/07ClusterConf.jpg)

### Cluster简易静态配置示例
![07ClusterConfEg](../ref/img/istio/07ClusterConfEg.jpg)

### L4过滤器tcp_proxy
![07L4_tcp_proxy](../ref/img/istio/07L4_tcp_proxy.jpg)

### TCP代理配置示例
![07tcp_proxy_eg](../ref/img/istio/07tcp_proxy_eg.jpg)

### 运行的示例
1. envoy.yaml
    ![07eg_envoy_yaml](../ref/img/istio/07eg_envoy_yaml.jpg)

1. docker-compose.yaml(模拟sidecar)
    ![07eg_docker-compose_yaml](../ref/img/istio/07eg_docker-compose_yaml.jpg)


## 34istio
 
 ### 介绍
![34intro](../ref/img/istio/34intro.jpg)

 ### 为什么
![34why](../ref/img/istio/34why.jpg)

 ### 功能
![34func](../ref/img/istio/34func.jpg)

 ### 是什么
![34what](../ref/img/istio/34what.jpg)

 ### 为什么要用istio
![34whyIstio](../ref/img/istio/34whyIstio.jpg)

 ### istio系统架构
![34arch](../ref/img/istio/34arch.jpg)

![34archGov](../ref/img/istio/34archGov.jpg)

### Pilot
![34pilot](../ref/img/istio/34pilot.jpg)

![34pilotArch](../ref/img/istio/34pilotArch.jpg)

### Mixer
![34mixer](../ref/img/istio/34mixer.jpg)

![34mixerMap](../ref/img/istio/34mixerMap.jpg)

### istio的安全模型
![34sec](../ref/img/istio/34sec.jpg)

### Citadel
![34citadel](../ref/img/istio/34citadel.jpg)

### Galley
![34galley](../ref/img/istio/34galley.jpg)

### Ingress Gateway 和 Egress Gateway
![34ingressEgress](../ref/img/istio/34ingressEgress.jpg)

### Sidecar Injector
![34injector](../ref/img/istio/34injector.jpg)

### 可视化
![34visual](../ref/img/istio/34visual.jpg)


## 35 istio install

### 本节话题
![35sub](../ref/img/istio/35sub.jpg)

### 示例集群
![35com](../ref/img/istio/35com.jpg)

### 快速安装
![35install](../ref/img/istio/35install.jpg)

// istioctl manifest generate --set profile=demo >/tmp/demo.yaml

// istioctl verify-install [-f /tmp/demo.yaml]



## 36istioDemo







