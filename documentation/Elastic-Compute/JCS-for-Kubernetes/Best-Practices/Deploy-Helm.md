# 部署Helm  
## 产品简介  
**1. Helm简介**  
Helm是一个包管理工具, 把Kubernetes资源(比如deployments、services或 ingress等) 打包到一个chart中，方便将其chart保存到chart仓库用来存储和分享, Helm支持发布应用配置的版本管理, 使发布可配置, 简化了Kubernetes部署应用的版本控制、打包、发布、删除、更新等操作。  
Kubernetes所发布的调查报告显示，其中有64%都是利用Helm，管理Kubernetes环境中执行的应用。Helm现在独立于Kubernetes，成为CNCF独立项目。    
**2. Helm架构图**  
![](https://github.com/jdcloudcom/cn/blob/edit/image/Elastic-Compute/JCS-for-Kubernetes/Helm架构图.png)  
 Helm架构由Helm客户端、Tiller服务器端和Chart仓库所组成；Tiller部署在Kubernetes中，Helm客户端从Chart仓库中获取Chart安装包，并将其安装部署到Kubernetes集群中。  
 **3. 产品功能**  
 Helm是管理Kubernetes包的工具，Helm能提供下面的能力：  
- 创建新的charts  
-	将charts打包成tgz文件  
-	与chart仓库交互  
-	安装和卸载Kubernetes的应用  
-	管理使用Helm安装的charts的生命周期    
**4. Helm组件**  
在Helm中有两个主要的组件，既Helm客户端和Tiller服务器：  
Helm客户端：这是一个供终端用户使用的命令行工具，客户端负责如下的工作：  
-	本地chart开发  
-	管理仓库  
-	与Tiller服务器交互  
-	发送需要被安装的charts  
-	请求关于发布版本的信息  
-	请求更新或者卸载已安装的发布版本  
Tiller服务器： Tiller服务部署在Kubernetes集群中，Helm客户端通过与Tiller服务器进行交互，并最终与Kubernetes API服务器进行交互。 Tiller服务器负责如下的工作：  
-	监听来自于Helm客户端的请求  
-	组合chart和配置来构建一个发布  
-	在Kubernetes中安装，并跟踪后续的发布  
-	通过与Kubernetes交互，更新或者chart  
客户端负责管理chart，服务器负责管理发布  

## 前提条件  
- 已经创建Kubernetes集群，请参考京东云Kubernetes集群入门指南[创建集群](https://docs.jdcloud.com/cn/jcs-for-kubernetes/create-to-cluster)  
- 以Centos7.4 64位为例进行部署，安装了Kubectl并连接Kubernetes集群，请参考入门指南[连接集群](https://docs.jdcloud.com/cn/jcs-for-kubernetes/connect-to-cluster)  
## 安装部署
1. 下载对应版本，目前京东云Kubernetes版本为1.8.12，请选择Helm版本为[2.7.2](https://github.com/helm/helm/releases?after=v2.8.0)  
`wget https://storage.googleapis.com/kubernetes-helm/helm-v2.7.2-linux-amd64.tar.gz`  
2. 解压缩  
 `tar -zxvf helm-v2.7.2-linux-amd64.tar.gz`  
3. 在解压后的目录中找到二进制文件，并将其移动到所需的位置
 `mv linux-amd64/helm /usr/local/bin/helm`
4. 运行以下命令  
 `helm help`  
5. 为Tiller添加权限，详见[Role-based Access Control](https://docs.helm.sh/using_helm/#role-based-access-control)，新建rbac-config.yaml，内容如下：  
```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
```  
   执行如下命令：  
   `kubectl create -f rbac-config.yaml`  
6. 初始化 Helm 并安装 Tiller 服务  
```
helm init --upgrade --service-account tiller --tiller-image registry.docker-cn.com/rancher/tiller:v2.7.2
```  
7. 运行以下命令  
`helm version`  
出现以下信息，确认安装成功  
```
Client: &version.Version{SemVer:"v2.7.2", GitCommit:"8478fb4fc723885b155c924d1c8c410b7a9444e6", GitTreeState:"clean"}
Server: &version.Version{SemVer:"v2.7.2", GitCommit:"8478fb4fc723885b155c924d1c8c410b7a9444e6", GitTreeState:"clean"}
```  
## 使用Helm
1. 首次安装Helm时，已预配置为与官方Kubernetes chart 存储库repo。该repo包含许多精心策划和维护的charts。此charts repo默认以stable命名。 
更新chart列表  
`helm repo update`  
查找所有可用的chart：  
`helm search`
```
NAME                                 	VERSION  	DESCRIPTION                                       
stable/acs-engine-autoscaler         	2.2.0    	Scales worker nodes within agent pools            
stable/aerospike                     	0.1.7    	A Helm chart for Aerospike in Kubernetes          
stable/anchore-engine                	0.2.6    	Anchore container analysis and policy evaluatio...
stable/apm-server                    	0.1.0    	The server receives data from the Elastic APM a...
stable/ark                           	1.2.2    	A Helm chart for ark                 
...
```  
搜索mysql相关的chart    
`helm search mysql`  
```
NAME                            	VERSION	DESCRIPTION                                       
stable/mysql                    	0.10.2 	Fast, reliable, scalable, and easy to use open-...
stable/mysqldump                	1.0.0  	A Helm chart to help backup MySQL databases usi...
stable/prometheus-mysql-exporter	0.2.1  	A Helm chart for prometheus mysql exporter with...
stable/percona                  	0.3.3  	free, fully compatible, enhanced, open source d...
stable/percona-xtradb-cluster   	0.3.0  	free, fully compatible, enhanced, open source d...
stable/phpmyadmin               	1.3.0  	phpMyAdmin is an mysql administration frontend    
stable/gcloud-sqlproxy          	0.6.0  	Google Cloud SQL Proxy                            
stable/mariadb                  	5.2.3  	Fast, reliable, scalable, and easy to use open-...
```  
查看chart信息  
`helm inspect stable/mysql`  
```
appVersion: 5.7.14
description: Fast, reliable, scalable, and easy to use open-source relational database
  system.
engine: gotpl
home: https://www.mysql.com/
icon: https://www.mysql.com/common/logos/logo-mysql-170x115.png
keywords:
- mysql
- database
- sql
maintainers:
- email: o.with@sportradar.com
  name: olemarkus
- email: viglesias@google.com
  name: viglesiasce
name: mysql
sources:
- https://github.com/kubernetes/charts
- https://github.com/docker-library/mysql
version: 0.10.2
...
```  
2. 安装软件包  
**- 部署WordPress**  
WordPress简介：  
WordPress 创建于 2003 年，逐渐发展成为世界上使用最多的自助博客工具。  
执行以下命令：  
`helm install stable/wordpress`  
输出以下信息：  
```
NAME:   boisterous-aardwolf
LAST DEPLOYED: Thu Nov  8 16:24:36 2018
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1beta1/Deployment
NAME                           DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
boisterous-aardwolf-wordpress  1        1        1           0          1s

==> v1beta1/StatefulSet
NAME                         DESIRED  CURRENT  AGE
boisterous-aardwolf-mariadb  1        1        1s
...
```
由于该部署需要云硬盘，需要创建PVC。  
输入以下命令：  
`kubectl get pvc`  
输出以下信息：  
···
NAME                                 STATUS    VOLUME    CAPACITY   ACCESS MODES   STORAGECLASS   AGE
boisterous-aardwolf-wordpress        Pending                                       default        2m
data-boisterous-aardwolf-mariadb-0   Pending                                       default        2m
···  
创建名称为boisterous-aardwolf-wordpress、data-boisterous-aardwolf-mariadb-0 的PVC：  
以boisterous-aardwolf-wordpress为例，创建boisterous-aardwolf-wordpress.yaml文件内容分别如下：  
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: boisterous-aardwolf-wordpress
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: jdcloud-ssd
  resources:
    requests:
      storage: 20Gi
```  
删除原有的boisterous-aardwolf-wordpress：  
`kubectl delete pvc boisterous-aardwolf-wordpress`  
执行创建：  
`kubectl create -f boisterous-aardwolf-wordpress.yaml`  
按照该方式创建命名为data-boisterous-aardwolf-mariadb-0的PVC。  
稍等几分钟，执行以下命令：  
`kubectl get pod`  
输出以下信息，为running状态，表示部署成功。  
```
NAME                                             READY     STATUS    RESTARTS   AGE
boisterous-aardwolf-mariadb-0                    1/1       Running   0          57m
boisterous-aardwolf-wordpress-7b94db45db-s4g8f   1/1       Running   0          57m
```  
执行以下命令  
`kubectl get svc`  
输出以下信息  
```
NAME                            TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                      AGE
boisterous-aardwolf-mariadb     ClusterIP      192.168.57.31    <none>         3306/TCP                     1h
boisterous-aardwolf-wordpress   LoadBalancer   192.168.60.113   114.67.94.77   80:31860/TCP,443:30346/TCP   1h
kubernetes                      ClusterIP      192.168.56.1     <none>         443/TCP                      2d
```  
其中114.67.94.77为外部访问IP，访问地址为：WordPress URL: http://114.67.94.77，显示以下信息：  
![](https://github.com/jdcloudcom/cn/blob/edit/image/Elastic-Compute/JCS-for-Kubernetes/WordPress1.png)  
WordPress Admin URL: http://114.67.94.77/admin  
![](https://github.com/jdcloudcom/cn/blob/edit/image/Elastic-Compute/JCS-for-Kubernetes/WordPress2.png)   
用户名：user  
密码：`$(kubectl get secret --namespace default boisterous-aardwolf-wordpress -o jsonpath="{.data.wordpress-password}" | base64 --decode)`  
![](https://github.com/jdcloudcom/cn/blob/edit/image/Elastic-Compute/JCS-for-Kubernetes/WordPress3.png)   

## 参考信息
1. 关于Helm的详细内容，还请[Helm官网](https://docs.helm.sh/)   
2. [Kubeapps Hub](https://hub.kubeapps.com)作为公共的chart应用仓库，目前以chart的格式提供Nginx、Jenkins、Redis等常用应用  