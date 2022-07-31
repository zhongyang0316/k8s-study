# 预习课程

### 阅读说明

有三个文档《docker基础》，《docker和k8s学习》和《Kubernetes安装手册》。

- 若之前没有docker的使用经验，建议学习一下《docker基础》
- 若之前有了解docker，想进一步了解k8s，可以尝试按照《安装手册》，尝试提前安装一下k8s集群
- 《docker和k8s学习》系统性了解容器技术之旅

### 环境准备

操作如下：

1. 各节点安装docker

   ```powershell
   ## 配置网卡转发,看值是否为1
   $ sysctl -a |grep -w net.ipv4.ip_forward
   net.ipv4.ip_forward = 1
   
   ## 若未配置，需要执行如下
   $ cat <<EOF >  /etc/sysctl.d/docker.conf
   net.bridge.bridge-nf-call-ip6tables = 1
   net.bridge.bridge-nf-call-iptables = 1
   net.ipv4.ip_forward=1
   EOF
   $ sysctl -p /etc/sysctl.d/docker.conf
   
   ## 下载阿里源repo文件
   $ curl -o /etc/yum.repos.d/Centos-7.repo http://mirrors.aliyun.com/repo/Centos-7.repo
   $ curl -o /etc/yum.repos.d/docker-ce.repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
   
   ## yum安装
   $ yum install -y docker-ce-18.09.9
   
   ## 配置源加速
   $ mkdir -p /etc/docker
   $ vi /etc/docker/daemon.json
   {
     "registry-mirrors" : [
       "https://dockerhub.azk8s.cn",
       "https://reg-mirror.qiniu.com",
       "https://registry.docker-cn.com",
       "https://ot2k4d59.mirror.aliyuncs.com/"
     ]
   }
   
   ## 设置开机自启
   $ systemctl enable docker  
   $ systemctl daemon-reload
   
   ## 启动docker
   $ systemctl start docker 
   ```

2. 下载镜像

   使用docker pull的方式，下载如下镜像到各节点

   ```powershell
   docker pull registry.aliyuncs.com/google_containers/kube-apiserver:v1.16.2
   docker pull registry.aliyuncs.com/google_containers/kube-controller-manager:v1.16.2
   docker pull registry.aliyuncs.com/google_containers/kube-scheduler:v1.16.2
   docker pull registry.aliyuncs.com/google_containers/kube-proxy:v1.16.2
   docker pull registry.aliyuncs.com/google_containers/pause:3.1
   docker pull registry.aliyuncs.com/google_containers/etcd:3.3.15-0
   docker pull registry.aliyuncs.com/google_containers/coredns:1.6.2
   quay.io/coreos/flannel:v0.11.0-amd64
   nginx:alpine
   kubernetesui/dashboard:v2.0.0-beta5
   kubernetesui/metrics-scraper:v1.0.1
   centos:centos7.5.1804
   mysql:5.7
   quay.io/kubernetes-ingress-controller/nginx-ingress-controller:0.28.0
   ```

   



