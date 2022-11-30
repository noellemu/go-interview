# Kubernetes 学习笔记

## Windows 安装 kind

使用 minikube 总是由于各种各样莫名其妙的原因启动失败，所以我换成了 kind，一次成功。

```powershell
# 安装 kubectl
winget install kubectl
# 安装 kind
go install sigs.k8s.io/kind@v0.17.0
# 启动 K8s 集群
kind create cluster
# 查看 K8s 启动状态
kubectl get pod -A
```
