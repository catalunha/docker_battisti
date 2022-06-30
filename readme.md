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
