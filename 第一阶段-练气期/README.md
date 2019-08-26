
### 目标

1. `Kubernetes` 背景
2. 安装 `Kubernetes`
3. `Kubernetes` 基本概念和使用方法

### （1）`Kubernetes` 背景

`Kubernetes` 是容器编排引擎产品。

学习`Kubernetes`如何编排容器，包括优化资源利用、高可用、滚动更新、网络插件、服务发现、监控、数据管理、日志管理等。

### （2）安装 `Kubernetes`


### （3）`Kubernetes` 基本概念和使用方法

#### 1. 集群（Cluster）

Cluster 是计算、存储和网络资源的集合，`Kubernetes`利用这些资源运行各种基于容器的应用。

如图：



#### 2. Master

Master 是 Cluster 的大脑，它的主要职责是调度，即决定将应用放在哪里运行。


#### 3. Node

Node 的职责是运行容器应用。Node 由 Master 管理，Node 负责监控并汇报容器的状态，同时根据 Master 的要求管理容器的生命周期。


#### 4. Pod

Pod 是 `Kubernetes`的最小工作单元。每个 Pod 包含一个或多个容器。Pod 中的容器会作为一个整体被 Master 调度到一个 Node 上运行。


#### 5. Controller

`Kubernetes` 通常不会直接创建 Pod， 而是通过 Controller 来管理 Pod的。

#### 6. Service

> Deployment 可以部署多个副本，每个 Pod 都有自己的IP。

Pod 可能会被频繁地销毁和重启，它们的 IP 会发生变化，用 IP 来访问不太现实。

`Kubernetes` Service 定义了外界访问一组特定 Pod 的方式。Service 有自己的 IP 和端口，Service 为 Pod 提供了负载均衡。


#### 7. Namespace

Namespace 可以将一个物理的 Cluster 逻辑上划分成多个虚拟 Cluster，每个 Cluster 就是一个 Namespace。不同 Namespace 里的资源是完全隔离的。



### 参考资料

1. https://github.com/caicloud/kube-ladder#%E7%AC%AC%E4%B8%80%E9%98%B6%E6%AE%B5-%E7%82%BC%E6%B0%94%E6%9C%9F2-4-%E5%91%A8%E6%AF%8F%E5%91%A8-3-5-%E5%B0%8F%E6%97%B6

2. http://dockone.io/article/932
