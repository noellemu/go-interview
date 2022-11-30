# Kubernetes 架构

> 面试频率：高
> ps：只要面试问了 Kubernetes，这部分内容面试基本上必问。

一个 Kubernetes 集群至少包含一个控制平面（`Control Pane`）以及一个或多个工作节点（`Worker Node`）。

![Kubernetes 架构图](https://www.topgoer.cn/uploads/blog/202205/attach_16ec72312dff591b.png)

> 图片来自 [Introduction to Kubernetes architecture](https://www.redhat.com/en/topics/containers/kubernetes-architecture)

## 控制平面（Control Pane / Master）

控制平面负责管理工作节点以及维护集群状态，所有的任务分配都来自于控制平面。控制平面会为集群做出全局决策，比如资源的调度、检测和相应集群事件。

### kube-apiserver

`kube-apiserver` 是 Kubernetes 控制平面的前端，负责响应内部和外部请求。它提供了资源操作的唯一入口，并且提供了认证授权、访问控制、API 注册与发现等机制。

可以通过 RESTful API（例如 Kubernetes Go Client）、`kubectl` 命令或其他工具（例如 `kubeadm`）来访问 Apiserver。

### kube-scheduler

`kube-scheduler` 负责资源的调度，它按照预定的调度策略将 Pod 调度到相应的机器上。Scheduler 会考虑 Pod 的资源需求（如 CPU、内存等）以及集群的运行状况。并将 Pod 安排到合适的计算节点上。

### kube-controller-manager

`kube-controller-manager` 负责维护集群的状态，例如故障检测、自动扩容、滚动更新等。Kubernetes controller-manager 是多个控制器的集合，包含：

- `Node Controller`：负责在 Node 出现故障时进行通知和响应。

- `Job Controller`：监测 Job 对象，并创建相应的 Pods 运行这些 Job。

- `Endpoints Controller`：填充 Endpoint 对象（加入 Service 和 Pod）。

- `Service Account & Token Controllers`：为新的 Namespace 创建默认账户和 API Access Token。

### etcd

`etcd` 保存了配置数据和整个集群的状态。

## 工作节点（Worker Node）

工作节点负责执行由控制平面分配的请求任务，运行实际的应用和工作负载。它负责维护运行的 Pod 并提供 Kubernetes 运行环境。

### kubelet

`kubelet` 会在集群中的每个 Node 上运行，负责维护容器的生命周期，并保证容器都运行在 Pod 中。当 Control Pane 需要在 Node 上执行某些操作时，kubelet 就会执行这些操作。

### kube-proxy

`kube-proxy` 是 Node 上运行的网络代理，负责为 Service 实现内部的服务发现和负载均衡。kube-proxy 维护节点网络规则和转发流量，实现集群内部或外部的网络与 Pod 的网络通信。

### Container Runtime Engine

`Container Runtime Engine` 是负责运行容器的软件，如 `Docker`、`Containerd` 或者其他实现了 `CRI (Container Runtime Interface)` 的软件。

### Pod

`Pod` 是 Kubernetes 模型中最小、最简单的单元，由一个或者多个紧密耦合的容器组成。
