# 설치
sudo apt-get update

sudo apt-get -y install apt-transport-https ca-certificates curl gnupg2 software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

sudo apt-get update

sudo apt-get install -y docker-ce docker-ce-cli containerd.io

sudo usermod -aG docker $USER

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF

sudo apt-get update

sudo apt-get install -y kubelet kubeadm kubectl

sudo apt-mark hold kubelet kubeadm kubectl



# kubeadm 실행
sudo kubeadm init

# kubeadm join 10.178.0.2:6443 --token 0e1mfu.gfu2fs8lxwjvfdf6 \
#    --discovery-token-ca-cert-hash sha256:187069ace2f7a4e302d60067ce8c360e0f033bb53252edbcf5dd408ac08aca7d 

# 토큰 재발급 (유효시간 24시간)
kubeadm toke create --print-join-command

# kubectl 명령문 사용할 수 있게 하기
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config



# Weave Net 애드온 설치 (클러스터마다 1개씩의 네트워크 애드온 설치가능, pod간 통신을 위해 설치)
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"

# pod 확인
watch kubectl get pods --all-namespaces

# Control-Plane Node에 pod를 올리기 위한 taint 해제작업
kubectl taint nodes --all node-role.kubernetes.io/master-

# 노드 확인
kubectl get nodes -o wide



# 방화벽 오픈
6443 : k8s API server
2379-2380 : etcd server client API
10250 : Kubelet API
10251 : kube-scheduler
10252 : kube-controller-manager
10250 : Kubelet API
30000-32767 : NodePort Services



# run nginx
kubectl run nginx-app --image nginx --port=80

# show pods
kubectl get pods

# expose
kubectl expose pod nginx-app --type=NodePort

# show service
kubectl get service

# delete pods
kubectl delete pod nginx-app

# delete service
kubectl delete svc nginx-app



# show namespace
kubectl get namespace

# run create namespace
kubectl appliy -f namespace.yaml

# run nginx using namespace
kubectl run nginx --image=nginx --namespace=team1

# show pod with namespace
kubectl get pod -n team1

# delete pod with namespace
kubectl delete pod nginx -n team1

# delete namespace
kubectl delete namespace team1


# run pod
kubectl apply -f pod.yaml

# view pod
kubectl describe pod nodejs-app


# show replicaset
kubectl get replicaset
kubectl get rs

# show deployment
kubectl get deployment


# rollout
kubectl rollout status deployment.apps/nodejs-app

# show rollout history
kubectl rollout history deployment.apps/nodejs-app

# write CHANGE-CAUSE in rollout history
kubectl annotate deployment.apps/nodejs-app kubernetes.io/change-cause="image updated to latest"

# undo rollout
kubectl rollout undo deployment.apps/nodejs-app

# undo rollout with revision
kubectl rollout undo deployement.apps/nodejs-app --to-revision=2


# expose deployment
kubectl expose deploy nodejs-app

# expose deployment external 
kubectl expose deploy nodejs-app --type=NodePort

# show service 
kubectl get svc nodejs-app

# delete expose(service)
kubectl delete svc nodejs-app




# install ingress (nginx)
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/nginx-0.29.0/deploy/static/mandatory.yaml

# expose ingress
kubectl expose deploy nginx-ingress-controller --type=NodePort -n ingress-nginx


# show persistent volume
kubectl get pv

# show persistent volume claim
kubectl get pvc


# run mysql (서비스가 있어야 mysql 호스트를 찾을 수 있음.)
kubectl run -it --rm --image=mysql:5.6 --restart=Never mysql-client -- mysql -h mysql -p1234