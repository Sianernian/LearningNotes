[TOC]

### 允许污点

```
kubectl taint nodes --all node-role.kubernetes.io/master-
```



### 禁止

```
kubectl taint nodes k8s node-role.kubernetes.io/master=true:NoSchedule
```



### Create a deployment

```
1、使用 kubectl create 创建pod       pod基于docker镜像进行运行一个容器

kubectl create deployment XXXX  --image=k8s.gcr.io/echoserver:1.4

2、查看 Deployment      查看pod    查看事件       

​    kubectl  get deployment       kubectl get pods     kubectl  get events          

​	查看配置

​      kubectl  get  view
```



### Create a Service 

```
   1、将pod 公开到网络上  expose

  kubectl  expose deployment  hello-node  --type=LoadBalancer --port=8080

​	2、 查看创建的服务

​		kubectl get services  

​		kubectl service hello-node
```



### 子节点 从master 复制config 文件  配置环境变量

```
echo "export KUBECONFIG=./config" >> /etc/profile
source /etc/profile
```

### Clean up

```
kubectl  delete service  hello-node 

kubectl delete deployment hello-node

kubectl delete pod -n   <name>  --force --grace-period=0 强制删除 node

```

​     

### 查看所有pod信息

```
kubectl get pods --all-namespaces -o wide

查看 pod 详情
kubectl describe pod  name。。。。
```



生成token   slave节点  使用token加入master

```
kubeadm token create --print-join-command  

在生成的token后加  --ignore-preflight-errors=Swap
```



### 查看日志

```
kubectl logs -f -n agent agent-665cb778fb-xnqxr agent --tail 100

​ 查看端口  netstat  -pnlt | grep 6443

kubeadm token create --ttl 0   创建永久token

kubeadm token list   查看token

kubectl config use-context XXXXXX      # 使用XXXXXX集群作为当前上下文
kubectl cluster-info                     # 查看集群信息
```



### 远端传输文件

```
链接
sftp  zxkfsftp@192.168.131.125 

scp  -r   root@ip:/复制路径  /粘贴路径

/data/nfs/agent/ ./
```

