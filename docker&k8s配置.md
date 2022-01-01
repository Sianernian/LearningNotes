```shell
[virtualbox]
name=Oracle Linux / RHEL / CentOS-$releasever / $basearch - VirtualBox
baseurl=http://download.virtualbox.org/virtualbox/rpm/el/$releasever/$basearch
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://www.virtualbox.org/download/oracle_vbox.asc


yum clean all && yum makecache
yum install VirtualBox-6.1

sudo cat <<EOF > /etc/yum.repos.d/kubernetes.repo

[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg

EOF

docker 删除旧版本
yum remove docker  docker-common docker-selinux docker-engine

安装需要的包
yum install -y yum-utils device-mapper-persistent-data lvm2

查看所有仓库中所有docker版本

yum list docker-ce --showduplicates | sort -r

yum install docker-ce-18.06.1.ce 


yum install -y kubelet-1.16.3 kubeadm-1.16.3 kubectl-1.16.3

chmod +x kubectl &&
mv kubectl /usr/local/bin
卸载管理组件
yum erase -y kubelet kubectl kubeadm

curl -Lo minikube http://kubernetes.oss-cn-hangzhou.aliyuncs.com/minikube/releases/v1.12.0/minikube-linux-amd64 

chmod +x minikube
sudo mv minikube /usr/local/bin/


docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-apiserver:v1.18.0
&& docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-controller-manager:v1.18.0
&& docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-scheduler:v1.18.0
&& docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-proxy:v1.18.0
&& docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/pause:3.2
&& docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/etcd:3.4.3-0
&& docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/coredns:1.6.7
&& docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/storage-provisioner:v1.8.1


改tag
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/kube-apiserver:v1.18.0  k8s.gcr.io/kube-apiserver:v1.18.0
&&docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/kube-controller-manager:v1.18.0  k8s.gcr.io/kube-controller-manager:v1.18.0
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/kube-scheduler:v1.18.0  k8s.gcr.io/kube-scheduler:v1.18.0 &&docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/kube-proxy:v1.18.0  k8s.gcr.io/kube-proxy:v1.18.0 &&docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/pause:3.2  k8s.gcr.io/pause:3.2 &&docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/etcd:3.4.3-0  k8s.gcr.io/etcd:3.4.3-0 &&docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/coredns:1.6.7  k8s.gcr.io/coredns:1.6.7 &&docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/storage-provisioner:v1.8.1  gcr.io/k8s-minikube/storage-provisioner:v1.8.1

docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-apiserver:v1.18.3 &&
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-scheduler:v1.18.3 &&
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-controller-manager:v1.18.3 &&
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/pause:3.2 &&
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/coredns:1.6.7 &&
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/etcd:3.4.3-0 
```

