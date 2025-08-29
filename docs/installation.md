# 1、环境准备

## 1.1 核心组件依赖版本

```shell
docker 20.10.23
containerd 1.7.13
k8s arm集群 1.21.14
Kubeflow pipeline 2.14
ml-metadata 1.14.0
```

## 1.2 环境基础配置

```shell
# 关闭防火墙
systemctl stop firewalld
systemctl disable firewalld

# 关闭 SELinux
setenforce 0
sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config

# 关闭 swap
swapoff -a
sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab

# 配置网络参数
cat <<EOF > /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
EOF
sysctl --system

# 加载必要的内核模块
cat <<EOF > /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF
modprobe overlay
modprobe br_netfilter

# 设置主机名（根据您的需要修改）
hostnamectl set-hostname k8s-master

```
# 2、安装docker

```shell
2.1	添加docker repo
2.2	通过yum安装指定版本docker 20.10.23
yum install -y docker-ce-3:20.10.23-3.el7 docker-ce-cli-3:20.10.23-3.el7 containerd.io
2.3	启动docker并查看docker状态
systemctl start docker
systemctl status docker
```

# 3、Containerd配置

```shell
# 生成默认配置文件
containerd config default > /etc/containerd/config.toml

# 修改配置文件以使用 systemd cgroup 驱动
sed -i 's/SystemdCgroup = false/SystemdCgroup = true/' /etc/containerd/config.toml

# 重启 Containerd 使配置生效
systemctl restart containerd
# 检查 Containerd 状态
systemctl status containerd
# 测试 Containerd
ctr version

```

# 4、安装 Kubernetes 组件
```shell
4.1 添加k8s仓库
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-aarch64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF
4.2	安装 kubeadm、kubelet 和 kubectl
# 安装指定版本
yum install -y kubelet-1.21.14-0 kubeadm-1.21.14-0 kubectl-1.21.14-0 --disableexcludes=kubernetes

# 启用 kubelet
systemctl enable kubelet
4.3	使用 kubeadm 初始化集群
# 初始化单节点集群
kubeadm init --kubernetes-version=v1.21.14 \
  --pod-network-cidr=10.244.0.0/16 \
  --apiserver-advertise-address=$(hostname -I | awk '{print $1}') \
  --cri-socket=/run/containerd/containerd.sock

# 初始化成功后，按照提示配置 kubectl
mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config
4.4	安装网络插件flannel
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
4.5	取消主节点的污点（单节点集群需要）
# 取消主节点的污点，使其可以运行 Pod
kubectl taint nodes --all node-role.kubernetes.io/master-
4.6	验证安装
# 检查节点状态
kubectl get nodes

# 检查所有 Pod 状态
kubectl get pods --all-namespaces

# 检查集群信息
kubectl cluster-info

```
# 5、Kubeflow plpeline安装
```shell
export PIPELINE_VERSION=2.14.0

kubectl apply -k "github.com/kubeflow/pipelines/manifests/kustomize/cluster-scoped-resources?ref=$PIPELINE_VERSION"
kubectl wait --for condition=established --timeout=60s crd/applications.app.k8s.io
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.18.2/cert-manager.yaml
kubectl wait --for=condition=Ready pod -l app.kubernetes.io/instance=cert-manager -n cert-manager --timeout=300s
kubectl apply -k "github.com/kubeflow/pipelines/manifests/kustomize/env/cert-manager/platform-agnostic-k8s-native?ref=$PIPELINE_VERSION"

```
# 6、ARM镜像制作
```shell
6.1	ARM镜像制作介绍
由于kubeflow pipeline官方默认只提供x86镜像，需要手动在arm环境上根据Dockerfile，修改适配在arm环境上重新构建镜像，构建成功之后，更换原来的官方镜像。Kubeflow pipeline一共有14个pod，里面包含需要替换的镜像有16个，下面通过介绍kubeflow pipeline server的arm镜像制作做示例。
6.2	Kubeflow pipeline server arm镜像制作
#克隆kubeflow pipeline 2.14仓库到本地
git clone -b 2.14.0 https://github.com/kubeflow/pipelines.git
cd pipelines
#查找Dockerfile的路径以及内容,server属于后端，所以会在backend路径下
ls backend
 
#查看Dockerfile内容，并适配arm版本
cat backend/Dockerfile
   
#将Dockerfie中amd64的包替换成arm64,然后重新构建
docker build -f backend/Dockerfile -t kfp-server:arm64 .

#构建成功之后需要推送到containerd
docker save kfp-server:arm64 -o kfp-server.tar
ctr -n=k8s.io images import kfp-server.tar
#先到在镜像容器的名字，再更换原来的x86镜像
Kubectl get deployment ml-pipeline -o jsonpath=’{.spec.template.spec.containers[*].name}’ -n kubeflow
 

Kubectl set image deployment/ml-pipeline ml-pipeline-api-server= kfp-server:arm64 -n Kubeflow
#如果报arm镜像拉不下来，认证错误，需要修改pod拉取规则由Always改成IfNotPresent
kubectl patch deployment ml-pipeline -n Kubeflow –type=’json’ -p=’[{“op”:”replace”,”path”:”spec/template/spec/containers/0/imagePullPolicy”,”value”:”IfNotPresent”}]’
#更新之后pod会自动重启，直至pod为running状态


```
