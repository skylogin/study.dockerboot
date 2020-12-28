# build docker image
docker build -t backend .

# docker run
docker run --name backend --link mysql:mysql -p 8088:8080 -d backend