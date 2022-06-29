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
