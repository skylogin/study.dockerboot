# build
docker build -t springboot .

# run
docker run --name app1 -d -p 80:8080 springboot

# view log
docker logs app1