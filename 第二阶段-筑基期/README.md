
## 一、目标

1. Kubernetes 的基本架构
2. Kubernetes 容器调度的基本流程

## 二、路径

### （1）`Kubernetes`的基本架构


#### 1. Master & Node

1. Master 是 Cluster 的大脑，它的主要职责是调度，即决定将应用放在哪里运行。
2. Node 的职责是运行容器应用。Node 由 Master 管理，Node 负责监控并汇报容器的状态，
   同时根据 Master 的要求管理容器的生命周期。

##### Master 与 Node 通讯

> 从集群到 Master 的通信路径都终止于 apiserver（其它 master 组件没有被设计为可暴
> 露远程服务）

从 master（apiserver）到集群有两种主要的通信路径。

1. 从 `apiserver` 到集群中每个节点上运行的 `kubelet` 进程。（使用 SSH）
2. 从 `apiserver` 通过它的代理功能到任何 node、pod 或者 service。（HTTP）


#### 2. Master 组件

##### 1. API Server

> 问题：`Kubernetes` 如何接收请求，又是如何将结果返回至客户端？


API Server 是 Master 节点启动运行后对外提供 Kubernetes API服务

主要职责：
1. 对外提供基于 RESTful 的管理接口
2. 配置资源
3. 实现认证、授权、准入控制等安全校验功能
4. 负责集群状态的存储操作（通过etcd）


资料：
1. https://www.zybuluo.com/dujun/note/64868
2. https://kubernetes.feisky.xyz/he-xin-yuan-li/index-1/apiserver

##### 2. Etcd

> 问题：了解 Etcd 主要功能机制

Etcd 提供 协同 和 存储配置服务。

Etch 使用的是 `Raft** 一致性算法来实现的。

Kubernetes系统中有两个服务用到 Etcd：
1. 网络插件 Flannel，对于其他网络插件也需要用到 Etcd 存储网络的配置信息
2. Kubernetes本身，包括各种对象的状态和元信息配置



资料：
1. https://jimmysong.io/kubernetes-handbook/concepts/etcd.html



##### 3. Controller Manager

> 问题：1. 了解控制器的原理2. List-Watch 的基本原理3. Kubernetes 默认情况下大致
>    包含哪些类型的控制器


1. 原理Controller Manager 由 kube-controller-manager 和 cloud-controller-manager
组成，是 Kubernetes 的大脑，它通过 apiserver 监控整个集群的状态，并确保集群处于
预期的工作状态。

**每个控制器只负责某种类型的特定资源**

控制器是一个控制循环，用来调节系统状态。


2. List-Watch 的基本原理

`ListWatcher` 是对某个特定命名空间中某个特定资源的 `list` 和 `watch` 函数的集合。

这样做有助于控制器只专注某种特定资源。


3. Kubernetes 默认情况下包含控制器
- ReplicaSet 控制器
- Endpoint 控制器
- Namespace 控制器
- Service Account 控制器


资料：
1. https://www.yangcs.net/posts/a-deep-dive-into-kubernetes-controllers/
2. https://jimmysong.io/kubernetes-handbook/concepts/controllers.html

##### 4. Scheduler

> 问题：1. Kubernetes 的调度流程是怎样的？2. 调度器在整个调度流程中的角色

默认配置情况下，Kubernetes调度器能够满足绝大多数要求，例如保证Pod只会被分配到资
源足够的节点上运行，把同一个集合的Pod分散在不同的计算节点上，平衡不同节点的资源
使用率等。

> 普通用户可以把Scheduler可理解为一个黑盒，黑盒的输入为待调度的Pod和全部计算节点
> 的信息，经过黑盒内部的调度算法和策略处理，输出为最优的节点，而后将Pod调度该节
> 点上 。


###### 1. Kubernetes 的调度流程是怎样的？

具体的调度过程，一般如下：
1. 首先，客户端通过API Server的REST API/kubectl/helm创建
   pod/service/deployment/job等，支持类型主要为JSON/YAML/helm tgz。
2. 接下来，API Server收到用户请求，存储到相关数据到etcd。
3. 调度器通过API Server查看未调度（bind）的Pod列表，循环遍历地为每个Pod分配节点，
   尝试为Pod分配节点。调度过程分为2个阶段：
- 第一阶段：预选过程，过滤节点，调度器用一组规则过滤掉不符合要求的主机。比如Pod
  指定了所需要的资源量，那么可用资源比Pod需要的资源量少的主机会被过滤掉。
- 第二阶段：优选过程，节点优先级打分，对第一步筛选出的符合要求的主机进行打分，在
  主机打分阶段，调度器会考虑一些整体优化策略，比如把容一个Replication Controller
  的副本分布到不同的主机上，使用最低负载的主机等。
4. 选择主机：选择打分最高的节点，进行binding操作，结果存储到etcd中。
5. 所选节点对于的kubelet根据调度结果执行Pod创建操作。


###### 2. 调度器在整个调度流程中的角色

决策者


###### 资料

1. http://dockone.io/article/2885



#### 3. Node 组件


##### 1. Kubelet

> 问题：Kubelet 是如何接受调度请求并启动容器的？


##### 2. Kube-proxy

> 问题：Kube-proxy 有什么作用，提供什么服务？

##### 3. Container Runtime

> 问题：了解都有哪些 Container Runtime，了解 Docker 一些基本操作与实现原理




#### 4. 核心 Addons & Plugins

##### 1. DNS


##### 2. Network Plugin


### （2）`Kubernetes` 容器调度的基本流程




## 三、资料

1. https://github.com/caicloud/kube-ladder#%E7%AC%AC%E4%B8%80%E9%98%B6%E6%AE%B5-%E7%82%BC%E6%B0%94%E6%9C%9F2-4-%E5%91%A8%E6%AF%8F%E5%91%A8-3-5-%E5%B0%8F%E6%97%B6

2. 
