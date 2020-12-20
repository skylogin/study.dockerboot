# build Dockerfile
docker build -t mynginx .

# run docker
docker run --name mynginx -p 8000:80 -d mynginx


# enter docker shell
docker exec -it mynginx /bin/bash

# copy between local and docker shell
docker cp mynginx:/usr/share/nginx/html/index.html index.html
docker cp index.html mynginx:/usr/share/nginx/html/

# rebuild or modify image
docker commit nginx mynginx

# show running process
docker ps

# show all process
docker ps -a

# stop process
docker stop mynginx

# delete process
docker rm mynginx

# delete image
docker rmi mynginx