# 설치
sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# 권한
sudo chmod +x /usr/local/bin/docker-compose


# (re)start
docker-compose up -d

