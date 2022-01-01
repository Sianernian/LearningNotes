操作步骤

  daemon.json  registries  改为 自己本机ip：5000

给 calico 和 kubeadm-images-1.16.3 镜像打tag   改为  ip:5000/name:version

改完 就 docker push   ip:5000/name:version



curl http://ip:5000/v2/_catalog   查看push 了 多少