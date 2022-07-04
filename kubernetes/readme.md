# GLossario
Relação de nomes entre Docker x Kubernets

Docker | Kubernets | Obs
---|---|---
image | |
container | |
volumes | |
 | Pods | Uma Vm para atender um chamado
 | service | conecta os pods a rede externa. expoe o pod para uso.



# Kubernets


Instalando Kubernets
https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-using-native-package-management

Instalando minikube
https://minikube.sigs.k8s.io/docs/start/

Executando o minikube
catalunha@pop-os:~/dockers/docker_battisti$ minikube version
minikube version: v1.26.0
commit: f4b412861bb746be73053c9f6d2895f12cf78565
catalunha@pop-os:~/dockers/docker_battisti$ minikube start --driver=docker
😄  minikube v1.26.0 on Debian bookworm/sid
✨  Using the docker driver based on user configuration
📌  Using Docker driver with root privileges
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
💾  Downloading Kubernetes v1.24.1 preload ...
    > preloaded-images-k8s-v18-v1...: 405.83 MiB / 405.83 MiB  100.00% 15.72 Mi
    > gcr.io/k8s-minikube/kicbase: 386.00 MiB / 386.00 MiB  100.00% 6.57 MiB p/
    > gcr.io/k8s-minikube/kicbase: 0 B [_________________________] ?% ? p/s 37s
🔥  Creating docker container (CPUs=2, Memory=4900MB) ...
🐳  Preparing Kubernetes v1.24.1 on Docker 20.10.17 ...
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🌟  Enabled addons: storage-provisioner, default-storageclass
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default

catalunha@pop-os:~/dockers/docker_battisti$ minikube status
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured

catalunha@pop-os:~/dockers/docker_battisti$ minikube stop
✋  Stopping node "minikube"  ...
🛑  Powering off "minikube" via SSH ...
🛑  1 node stopped.

catalunha@pop-os:~/dockers/docker_battisti$ minikube status
minikube
type: Control Plane
host: Stopped
kubelet: Stopped
apiserver: Stopped
kubeconfig: Stopped

catalunha@pop-os:~/dockers/docker_battisti$ minikube start --driver=docker

catalunha@pop-os:~/dockers/docker_battisti$ minikube dashboard

## modo imperativo
é igual do dockerfile e criação de containers independentes

# criando a imagem do projeto

catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto$ docker build -t catalunha/kubprojeto:1.0 .

catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto$ docker container ls
CONTAINER ID   IMAGE                                 COMMAND                  CREATED          STATUS          PORTS                                                                                                                                  NAMES
4e0ef8bcacae   catalunha/kubprojeto:1.0              "python ./app.py"        32 minutes ago   Up 32 minutes   0.0.0.0:5000->5000/tcp, :::5000->5000/tcp                                                                                              kubprojeto
b35308acf4bf   gcr.io/k8s-minikube/kicbase:v0.0.32   "/usr/local/bin/entr…"   5 hours ago      Up 5 hours      127.0.0.1:49162->22/tcp, 127.0.0.1:49161->2376/tcp, 127.0.0.1:49160->5000/tcp, 127.0.0.1:49159->8443/tcp, 127.0.0.1:49158->32443/tcp   minikube

catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto$ docker push catalunha/kubprojeto:1.0 


catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto$ kubectl create deployment kubprojeto-deploy --image=catalunha/kubprojeto:1.0
deployment.apps/kubprojeto-deploy created

catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto$ kubectl get deployments
NAME                READY   UP-TO-DATE   AVAILABLE   AGE
kubprojeto-deploy   1/1     1            1           4m56s


catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto$ kubectl describe deployments
Name:                   kubprojeto-deploy
Namespace:              default
CreationTimestamp:      Fri, 01 Jul 2022 17:11:11 -0300
Labels:                 app=kubprojeto-deploy
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=kubprojeto-deploy
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=kubprojeto-deploy
  Containers:
   kubprojeto:
    Image:        catalunha/kubprojeto:1.0
    Port:         <none>
    Host Port:    <none>
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   kubprojeto-deploy-6bf7c6d5d (1/1 replicas created)
Events:
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  5m30s  deployment-controller  Scaled up replica set kubprojeto-deploy-6bf7c6d5d to 1

catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto$ kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
kubprojeto-deploy-6bf7c6d5d-fdckw   1/1     Running   0          7m58s

catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto$ kubectl config view
apiVersion: v1
clusters:
- cluster:
    certificate-authority: /home/catalunha/.minikube/ca.crt
    extensions:
    - extension:
        last-update: Fri, 01 Jul 2022 12:20:39 -03
        provider: minikube.sigs.k8s.io
        version: v1.26.0
      name: cluster_info
    server: https://192.168.49.2:8443
  name: minikube
contexts:
- context:
    cluster: minikube
    extensions:
    - extension:
        last-update: Fri, 01 Jul 2022 12:20:39 -03
        provider: minikube.sigs.k8s.io
        version: v1.26.0
      name: context_info
    namespace: default
    user: minikube
  name: minikube
current-context: minikube
kind: Config
preferences: {}
users:
- name: minikube
  user:
    client-certificate: /home/catalunha/.minikube/profiles/minikube/client.crt
    client-key: /home/catalunha/.minikube/profiles/minikube/client.key

catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto$ kubectl expose deployment kubprojeto-deploy --type=LoadBalancer --port=5000
service/kubprojeto-deploy exposed

catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto$ minikube service kubprojeto-deploy
|-----------|-------------------|-------------|---------------------------|
| NAMESPACE |       NAME        | TARGET PORT |            URL            |
|-----------|-------------------|-------------|---------------------------|
| default   | kubprojeto-deploy |        5000 | http://192.168.49.2:30353 |
|-----------|-------------------|-------------|---------------------------|
🎉  Opening service default/kubprojeto-deploy in default browser...
catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto$ Opening in existing browser session.

catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto$ kubectl get services
NAME                TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
kubernetes          ClusterIP      10.96.0.1        <none>        443/TCP          5h53m
kubprojeto-deploy   LoadBalancer   10.107.102.184   <pending>     5000:30353/TCP   7m47s

catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto$ kubectl describe services/kubprojeto-deploy
Name:                     kubprojeto-deploy
Namespace:                default
Labels:                   app=kubprojeto-deploy
Annotations:              <none>
Selector:                 app=kubprojeto-deploy
Type:                     LoadBalancer
IP Family Policy:         SingleStack
IP Families:              IPv4
IP:                       10.107.102.184
IPs:                      10.107.102.184
Port:                     <unset>  5000/TCP
TargetPort:               5000/TCP
NodePort:                 <unset>  30353/TCP
Endpoints:                172.17.0.5:5000
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>

catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto$ kubectl scale deployment/kubprojeto-deploy --replicas=5
deployment.apps/kubprojeto-deploy scaled
catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto$ kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
kubprojeto-deploy-6bf7c6d5d-2jmf5   1/1     Running   0          16s
kubprojeto-deploy-6bf7c6d5d-6hvsv   1/1     Running   0          16s
kubprojeto-deploy-6bf7c6d5d-8ddsb   1/1     Running   0          16s
kubprojeto-deploy-6bf7c6d5d-fdckw   1/1     Running   0          81m
kubprojeto-deploy-6bf7c6d5d-lz4h7   1/1     Running   0          16s

catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto$ kubectl get rs
NAME                          DESIRED   CURRENT   READY   AGE
kubprojeto-deploy-6bf7c6d5d   5         5         5       84m

catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto$ kubectl scale deployment/kubprojeto-deploy --replicas=3
deployment.apps/kubprojeto-deploy scaled
catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto$ kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
kubprojeto-deploy-6bf7c6d5d-2jmf5   1/1     Running   0          5m59s
kubprojeto-deploy-6bf7c6d5d-fdckw   1/1     Running   0          86m
kubprojeto-deploy-6bf7c6d5d-lz4h7   1/1     Running   0          5m59s

catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto$ docker image build -t catalunha/kubprojeto:1.1 .

catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto$ docker push catalunha/kubprojeto:1.1

catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto$ kubectl set image deployment/kubprojeto-deploy kubprojeto=catalunha/kubprojeto:1.1
deployment.apps/kubprojeto-deploy image updated

catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto$ kubectl get services
NAME                TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
kubernetes          ClusterIP      10.96.0.1        <none>        443/TCP          6h52m
kubprojeto-deploy   LoadBalancer   10.107.102.184   <pending>     5000:30353/TCP   66m
catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto$ kubectl delete service kubprojeto-deploy
service "kubprojeto-deploy" deleted

catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto$ kubectl get deployments
NAME                READY   UP-TO-DATE   AVAILABLE   AGE
kubprojeto-deploy   3/3     3            3           122m
catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto$ kubectl delete deployment kubprojeto-deploy
deployment.apps "kubprojeto-deploy" deleted

catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto$ kubectl get deployments
No resources found in default namespace.
catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto$ kubectl get pods
No resources found in default namespace.

## modo declarativo

é igual ao docker-compose

criar arquivo de kub-declarativo.yml

limpar containers antigos
catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto-declarativo$ docker container ls
CONTAINER ID   IMAGE                                 COMMAND                  CREATED       STATUS       PORTS                                                                                                                                  NAMES
4e0ef8bcacae   catalunha/kubprojeto:1.0              "python ./app.py"        3 hours ago   Up 3 hours   0.0.0.0:5000->5000/tcp, :::5000->5000/tcp                                                                                              kubprojeto
b35308acf4bf   gcr.io/k8s-minikube/kicbase:v0.0.32   "/usr/local/bin/entr…"   7 hours ago   Up 7 hours   127.0.0.1:49162->22/tcp, 127.0.0.1:49161->2376/tcp, 127.0.0.1:49160->5000/tcp, 127.0.0.1:49159->8443/tcp, 127.0.0.1:49158->32443/tcp   minikube
catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto-declarativo$ docker container stop 4e0ef8bcacae


Retornando ao curso em 04-07

catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto-declarativo$ minikube start
😄  minikube v1.26.0 on Debian bookworm/sid
✨  Using the docker driver based on existing profile
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
🔄  Restarting existing docker container for "minikube" ...
🐳  Preparing Kubernetes v1.24.1 on Docker 20.10.17 ...
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
    ▪ Using image kubernetesui/dashboard:v2.6.0
    ▪ Using image kubernetesui/metrics-scraper:v1.0.8
🌟  Enabled addons: default-storageclass, storage-provisioner, dashboard
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default


catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto-declarativo$ kubectl apply -f kubprojetodeclarativo.yml 
deployment.apps/kubprojectdeclarative created
catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto-declarativo$ kubectl get pods
NAME                                     READY   STATUS    RESTARTS   AGE
kubprojectdeclarative-6bc654997f-7vmjl   1/1     Running   0          10s
kubprojectdeclarative-6bc654997f-c5rfg   1/1     Running   0          10s
kubprojectdeclarative-6bc654997f-n6tkq   1/1     Running   0          10s
kubprojectdeclarative-6bc654997f-r9gdv   1/1     Running   0          10s
catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto-declarativo$ 


catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto-declarativo$ minikube dashboard
🤔  Verifying dashboard health ...
🚀  Launching proxy ...
🤔  Verifying proxy health ...
🎉  Opening http://127.0.0.1:34415/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/ in your default browser...
Opening in existing browser session.


catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto-declarativo$ kubectl apply -f kubprojdec-service.yml 
service/kubprojdec-service created

catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto-declarativo$ kubectl get services
NAME                 TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
kubernetes           ClusterIP      10.96.0.1       <none>        443/TCP          3d4h
kubprojdec-service   LoadBalancer   10.98.141.225   <pending>     5000:32521/TCP   10s
catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto-declarativo$ minikube service kubprojdec-service
|-----------|--------------------|-------------|---------------------------|
| NAMESPACE |        NAME        | TARGET PORT |            URL            |
|-----------|--------------------|-------------|---------------------------|
| default   | kubprojdec-service |        5000 | http://192.168.49.2:32521 |
|-----------|--------------------|-------------|---------------------------|
🎉  Opening service default/kubprojdec-service in default browser...
catalunha@pop-os:~/dockers/docker_battisti/kubernetes/projeto-declarativo$ Opening in existing browser session.

catalunha@pop-os:~/dockers/docker_battisti/kubernetes/join$ kubectl apply -f kubproj-join.yml 
service/kubprojdec-service created
deployment.apps/kubprojectdeclarative created
catalunha@pop-os:~/dockers/docker_battisti/kubernetes/join$ kubectl get services
NAME                 TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
kubernetes           ClusterIP      10.96.0.1        <none>        443/TCP          3d5h
kubprojdec-service   LoadBalancer   10.100.146.117   <pending>     5000:31567/TCP   12s
catalunha@pop-os:~/dockers/docker_battisti/kubernetes/join$ kubectl get pods
NAME                                    READY   STATUS    RESTARTS   AGE
kubprojectdeclarative-5f7c86d54-klc7z   1/1     Running   0          16s
kubprojectdeclarative-5f7c86d54-sfgn2   1/1     Running   0          16s
kubprojectdeclarative-5f7c86d54-vf9gt   1/1     Running   0          16s
kubprojectdeclarative-5f7c86d54-xtgs2   1/1     Running   0          16s
catalunha@pop-os:~/dockers/docker_battisti/kubernetes/join$ minikube service kubprojdec-service
|-----------|--------------------|-------------|---------------------------|
| NAMESPACE |        NAME        | TARGET PORT |            URL            |
|-----------|--------------------|-------------|---------------------------|
| default   | kubprojdec-service |        5000 | http://192.168.49.2:31567 |
|-----------|--------------------|-------------|---------------------------|
🎉  Opening service default/kubprojdec-service in default browser...
catalunha@pop-os:~/dockers/docker_battisti/kubernetes/join$ Opening in existing browser session.

catalunha@pop-os:~/dockers/docker_battisti/kubernetes/join$ kubectl delete -f kubproj-join.yml 
service "kubprojdec-service" deleted
deployment.apps "kubprojectdeclarative" deleted

