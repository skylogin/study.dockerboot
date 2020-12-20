# run mysql 
docker run --name db -e MYSQL_ROOT_PASSWORD=1234 -p 3306:3306 -d mysql:5.7.28