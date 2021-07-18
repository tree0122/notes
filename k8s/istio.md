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

