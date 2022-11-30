# Pod

## 概念和特征

`Pod` 是包含一个或多个容器的容器组，是 Kubernetes 中创建和管理的最小对象。

- Pod 是 Kubernetes 中的最小调度单位，Kubernetes 通过管理 Pod 来管理容器。

- 同一个 Pod 的容器总是会被调度到同一个 Node 上，并且一起调度。

- Pod 中的容器共享存储（Volume）、网络和配置声明（如资源限制等），可以理解为运行特定程序的逻辑主机。

- 每个 Pod 有唯一的 IP 地址，共享同一个 Network Namespace，在同一个 Pod 内的不同容器共享同一个 IP 地址和端口空间，可以使用 localhost 相互通信（Pod 内容器共享同一个网络这一特点也是 Istio 实现的原理之一）。

可以通过 `kubectl exec -it podname -- /bin/bash` 进入一个 Pod。

## Infra Container

Pod 的实现需要使用一个叫做 `Infra` 的、对其他容器透明的中间容器。在 Pod 中，Infra 永远是第一个被创建的容器，其他容器则通过加入 Network Namespace 的方式与 Infra 容器相关联。Infra 容器使用的是一个占用极少资源的特殊镜像，叫做 `k8s.gcr.io/pause`。Pod 的生命周期与 Infra Container 一致。
