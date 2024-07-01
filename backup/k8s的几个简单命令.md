记录了k8s的几个简单命令

```
kubectl命令格式为：kubectl 动作 资源

# Deployment是k8s中用来管理发布的控制器
kubectl get deployment
kubectl get namespaces

# Pod是k8s的最小单位，里面包含一组容器
kubectl get pods

# 查看pod详情
kubectl describe pod $POD_NAME

-- 查看日志
kubectl logs $POD_NAME
```
