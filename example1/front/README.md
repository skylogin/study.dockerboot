# build docker image
docker build -t front .

# docker run
docker run --name front -p 8000:80 -d front