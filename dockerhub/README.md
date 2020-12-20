# 태그부여
docker tag mynginx skylogin/custom-nginx:mynginx

# 도커허브에 업로드
docker push skylogin/custom-nginx



# 레지스트리 직접구축
docker pull registry

# 실행
docker run -d -p 5000:5000 --restart=always --name registry registry

# 우분투 이미지 받기
docker pull ubuntu

# 태깅
docker image tag ubuntu localhost:5000/ubuntu

# GCP에서 방화벽 허용(5000포트)

# 로컬: 도커 레지스트리 다운 (에러남)
docker pull 34.64.198.252:5000/ubuntu

# 로컬: 도커 셋팅 > 도커엔진 > insecure-registries에 ip:port 추가 (34.64.198.252:5000)

# 로컬: 도커 레지스트리 다운 (성공)
docker pull 34.64.198.252:5000/ubuntu

