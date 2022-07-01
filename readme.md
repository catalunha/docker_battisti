https://github.com/matheusbattisti/curso_docker

# image01


# volumes-my
catalunha@pop-os:~/dockers/docker_battisti/volumes-my$ docker image build -t phpmessages .
catalunha@pop-os:~/dockers/docker_battisti/volumes-my$ docker container run -d -p 80:80 --name phpmessages phpmessages:latest
catalunha@pop-os:~/dockers/docker_battisti/volumes-my$ docker container run -d --rm -p 80:80 -v /dataphpmsg --name phpmessages phpmessages:latest
catalunha@pop-os:~/dockers/docker_battisti/volumes-my$ docker container run -d --rm -p 80:80 -v namevolume:/var/www/html/messages --name phpmessages phpmessages:latest
catalunha@pop-os:~/dockers/docker_battisti/volumes-my$ docker container run -d --rm -p 81:80 -v namevolume:/var/www/html/messages --name phpmessages2 phpmessages:latest
catalunha@pop-os:~/dockers/docker_battisti/volumes-my$ docker container run -d --rm -p 80:80 -v $(pwd)/messages:/var/www/html/messages --name phpmessages phpmessages:latest
catalunha@pop-os:~/dockers/docker_battisti/volumes-my$ sudo chown -R www-data:www-data messages
catalunha@pop-os:~/dockers/docker_battisti/volumes-my$ docker volume create volume-teste

# networks-my
catalunha@pop-os:~/dockers/docker_battisti/networks-my/externa$ docker image build -t flaskexterna .
catalunha@pop-os:~/dockers/docker_battisti/networks-my/externa$ docker run --rm -d -p 5000:5000 --name flaskexterna flaskexterna

## container
catalunha@pop-os:~/dockers/docker_battisti/networks-my/container/mysql$ docker image build -t mysqlnetworkapi .

catalunha@pop-os:~/dockers/docker_battisti/networks-my/container/mysql$ docker network create flasknetwork

catalunha@pop-os:~/dockers/docker_battisti/networks-my/container/mysql$ docker container run -d --rm  --name mysql_api_container --network flasknetwork -e MYSQL_ALLOW_EMPTY_PASSWORD=True mysqlnetworkapi

catalunha@pop-os:~/dockers/docker_battisti/networks-my/container/flask$ docker image build -t flaskapinetwork .

catalunha@pop-os:~/dockers/docker_battisti/networks-my/container/flask$ docker container run -d --rm -p 5000:5000 --name flask_api_network --network flasknetwork flaskapinetwork

Conectando rede apos run do container
Iniciando containers sem network

`catalunha@pop-os:~/dockers/docker_battisti$ docker container run -d --rm --name mysql_api_container -e MYSQL_ALLOW_EMPTY_PASSWORD=True mysqlnetworkapi
994757d7307123bbaa7a23836824c0336ecb7bab0584b1f90c8f8d653c73e498`

`catalunha@pop-os:~/dockers/docker_battisti$ docker container run -d --rm --name flaskapinetwork -p 5000:5000 flaskapinetwork
06819d8712b4f59eaeb3f759b441534289fa516b963592578a2f41c0c1f814d8`

Associando network após container running

`catalunha@pop-os:~/dockers/docker_battisti$ docker network connect flasknetwork 06819d8712b4`

`catalunha@pop-os:~/dockers/docker_battisti$ docker network connect flasknetwork 994757d73071`

# compose
catalunha@pop-os:~/dockers/docker_battisti/compose$ docker-compose up

# compose4
catalunha@pop-os:~/dockers/docker_battisti/compose4/flask$ docker image build -t flaskcompose .

catalunha@pop-os:~/dockers/docker_battisti/compose4/mysql$ docker image build -t mysqlcompose .

catalunha@pop-os:~/dockers/docker_battisti/compose4$ docker-compose up


# compose5
ao inves de buildar cada imagem separada faz o build junto com o compose.


catalunha@pop-os:~/dockers/docker_battisti/compose5$ docker-compose up -d

como visto é possivel, mas o build da imagem junto como compose é uma grande etapa que deve ser feita antes do compose.


# compose6
compartilhando a pasta do app.py para que as alterações no codigo reflitam em tempo real na aplicação.


# swarm
Aula 125
https://www.udemy.com/course/docker-para-desenvolvedores-com-docker-swarm-e-kubernetes/learn/lecture/25397284#questions/16686476
criadas as 3 instancias

catalunha@pop-os:~/myapp/0keys/aws$ ssh -i "nodeswarm01.pem" ec2-user@ec2-18-207-128-245.compute-1.amazonaws.com
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0777 for 'nodeswarm01.pem' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "nodeswarm01.pem": bad permissions
ec2-user@ec2-18-207-128-245.compute-1.amazonaws.com: Permission denied (publickey,gssapi-keyex,gssapi-with-mic).

catalunha@pop-os:~/myapp/0keys/aws$ sudo chmod 600 nodeswarm01.pem
[sudo] password for catalunha: 
catalunha@pop-os:~/myapp/0keys/aws$ ssh -i "nodeswarm01.pem" ec2-user@ec2-18-207-128-245.compute-1.amazonaws.com

       __|  __|_  )
       _|  (     /   Amazon Linux 2 AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-2/
[ec2-user@ip-172-31-20-21 ~]$ 

[ec2-user@ip-172-31-20-21 ~]$ sudo yum update -y

[ec2-user@ip-172-31-20-21 ~]$ sudo yum install docker

[ec2-user@ip-172-31-20-21 ~]$ sudo docker ps
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
[ec2-user@ip-172-31-20-21 ~]$ sudo service docker start
Redirecting to /bin/systemctl start docker.service
[ec2-user@ip-172-31-20-21 ~]$ sudo docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[ec2-user@ip-172-31-20-21 ~]$ 

[ec2-user@ip-172-31-20-21 ~]$ sudo usermod -a -G docker ec2-user
[ec2-user@ip-172-31-20-21 ~]$ sudo docker info
Client:
...

Instalação do docker na VM
```
sudo yum update -y
sudo yum install docker
sudo service docker start
start docker.service
sudo usermod -a -G docker ec2-user
sudo docker ps
sudo docker info
```


[ec2-user@ip-172-31-20-21 ~]$ sudo docker swarm init
Swarm initialized: current node (qn9uptqr9x2adi7s72t0h4jro) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-227hqxfpmvh3921jsbxovmhu3p7normgzb1ck0o7ljarzlyu0w-0m96ui7nwt2dj3n0isvv3poao 172.31.20.21:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.

[ec2-user@ip-172-31-20-21 ~]$ 

[ec2-user@ip-172-31-20-21 ~]$ docker node ls
ID                            HOSTNAME                       STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
znhiuys8z22h4kpcd7fcwsdpd *   ip-172-31-20-21.ec2.internal   Ready     Active         Leader           20.10.13
[ec2-user@ip-172-31-20-21 ~]$ 

[ec2-user@ip-172-31-20-21 ~]$ docker node ls
ID                            HOSTNAME                        STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
k2hdpyrgyeffquw5eqebdnxhd *   ip-172-31-20-21.ec2.internal    Ready     Active         Leader           20.10.13
zp78byrmkky4l1asc3un4c587     ip-172-31-30-177.ec2.internal   Ready     Active                          20.10.13
[ec2-user@ip-172-31-20-21 ~]$ ls
[ec2-user@ip-172-31-20-21 ~]$ 

Gerou um erro e voltei na VM manager e gerei no token e fui novamente em cada maqui e deu conexão. como a seguir:
[ec2-user@ip-172-31-20-21 ~]$ sudo docker swarm leave -f
Node left the swarm.
[ec2-user@ip-172-31-20-21 ~]$ sudo docker swarm init
Swarm initialized: current node (7q3suvvdp4b21ro0ujb8114kw) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-2h3b8hvxn226bf1uf1p5qt5ibjwib6pjmm1715m3npu54obbrt-9hqc1j0m6ltrrx5sdauok1vcj 172.31.20.21:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.


[ec2-user@ip-172-31-30-177 ~]$     docker swarm join --token SWMTKN-1-2h3b8hvxn226bf1uf1p5qt5ibjwib6pjmm1715m3npu54obbrt-9hqc1j0m6ltrrx5sdauok1vcj 172.31.20.21:2377
This node joined a swarm as a worker.
[ec2-user@ip-172-31-30-177 ~]$ 

[ec2-user@ip-172-31-30-177 ~]$     docker swarm join --token SWMTKN-1-2h3b8hvxn226bf1uf1p5qt5ibjwib6pjmm1715m3npu54obbrt-9hqc1j0m6ltrrx5sdauok1vcj 172.31.20.21:2377
This node joined a swarm as a worker.
[ec2-user@ip-172-31-30-177 ~]$ 

[ec2-user@ip-172-31-20-21 ~]$ docker node ls
ID                            HOSTNAME                        STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
7q3suvvdp4b21ro0ujb8114kw *   ip-172-31-20-21.ec2.internal    Ready     Active         Leader           20.10.13
f30f4ha97o793hkgsftfmwi24     ip-172-31-30-177.ec2.internal   Ready     Active                          20.10.13
ra8axzjd93natalyvwpo1p2z0     ip-172-31-30-177.ec2.internal   Ready     Active                          20.10.13
[ec2-user@ip-172-31-20-21 ~]$ 

Aplicar este comando em cada VM
`sudo docker swarm leave -f`
Ir na VM_Manager e pegar o ip publico 
Depois recriar o token
`docker swarm init --advertise-addr 18.207.128.245`
Aplicar token em cada maquina: 

[ec2-user@ip-172-31-20-21 ~]$ docker service create --name nginxswarm01 -p 80:80 nginx

[ec2-user@ip-172-31-20-21 ~]$ docker service ls
ID             NAME         MODE         REPLICAS   IMAGE          PORTS
tjojc6ues59u   nginxswarm   replicated   1/1        nginx:latest   *:80->80/tcp
[ec2-user@ip-172-31-20-21 ~]$ docker service rm tjojc6ues59u

[ec2-user@ip-172-31-20-21 ~]$ docker service create --name nginxreplicas --replicas 3 -p 80:80 nginx
otst5hmy9qggw1si8ykzjm5t3
overall progress: 3 out of 3 tasks 
1/3: running   [==================================================>] 
2/3: running   [==================================================>] 
3/3: running   [==================================================>] 

testando em cada ip vemos que o serviço foi replicado e esta rodando.

se pararmos uma replica o manager reativa o container
[ec2-user@ip-172-31-30-177 ~]$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS     NAMES
ce099361621c   nginx:latest   "/docker-entrypoint.…"   7 minutes ago   Up 7 minutes   80/tcp    nginxreplicas.3.vw74ldrxii5drju333tknthet
[ec2-user@ip-172-31-30-177 ~]$ docker container rm ce099361621c -f
ce099361621c
[ec2-user@ip-172-31-30-177 ~]$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[ec2-user@ip-172-31-30-177 ~]$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[ec2-user@ip-172-31-30-177 ~]$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[ec2-user@ip-172-31-30-177 ~]$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS                  PORTS     NAMES
1d3d38c72795   nginx:latest   "/docker-entrypoint.…"   6 seconds ago   Up Less than a second   80/tcp    nginxreplicas.3.caw4u9c64gglxlg2lg9v8plfy
[ec2-user@ip-172-31-30-177 ~]$ 

Solicitar o token novamente para criar outra VM para o manager
[ec2-user@ip-172-31-20-21 ~]$ docker swarm join-token manager
To add a manager to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-27q9kgdhz4t84zhl18by04re94gpuvlfpdet2dxsofoc57nzla-1va157vxp9rosbnvwgyfjenio 18.207.128.245:2377

[ec2-user@ip-172-31-20-21 ~]$ 

Retirar uma VM do node do swarm


[ec2-user@ip-172-31-20-21 ~]$ docker node ls
ID                            HOSTNAME                        STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
iwgvdp32ds8fhm62dy40zi69z     ip-172-31-19-124.ec2.internal   Ready     Active                          20.10.13
m2q5w5apgiscsj9gokjk2umz4 *   ip-172-31-20-21.ec2.internal    Ready     Active         Leader           20.10.13
w3en065o1mq7wxi7olpeddaue     ip-172-31-30-177.ec2.internal   Ready     Active                          20.10.13

[ec2-user@ip-172-31-19-124 ~]$ docker swarm leave
Node left the swarm.

[ec2-user@ip-172-31-20-21 ~]$ docker node ls
ID                            HOSTNAME                        STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
iwgvdp32ds8fhm62dy40zi69z     ip-172-31-19-124.ec2.internal   Down      Active                          20.10.13
m2q5w5apgiscsj9gokjk2umz4 *   ip-172-31-20-21.ec2.internal    Ready     Active         Leader           20.10.13
w3en065o1mq7wxi7olpeddaue     ip-172-31-30-177.ec2.internal   Ready     Active                          20.10.13
[ec2-user@ip-172-31-20-21 ~]$ 

Nao deixar o terminal do server cair
[ec2-user@ip-172-31-20-21 ~]$ nano ~/.ssh/config
ServerAliveInterval 50


Swarm com compose
[ec2-user@ip-172-31-20-21 ~]$ nano docker-compose.yml
[ec2-user@ip-172-31-20-21 ~]$ cat docker-compose.yml 
version: '3.3'
services:
  web:
    image: nginx
    ports:
      - 81:80  
[ec2-user@ip-172-31-20-21 ~]$ sudo docker stack deploy -c docker-compose.yml nginxcompose
Creating service nginxcompose_web
[ec2-user@ip-172-31-20-21 ~]$ 

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
