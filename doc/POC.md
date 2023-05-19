# POC Guide

Після вибору інструменту для розгортання kubernets кластеру зупинився на k3d. 
k3d розготався на віртуальній машині з ubuntu server 22.04. Додатково налаштовано і встановлено kubectl.
```
  curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
  sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
  kubectl version --client
  
  # Cluster create
  
  wget -q -O - https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
  k3d cluster create mycluster

```

## Встановлення ARGOCD
Наступним етапом буде розгортання системи ArgoCD в кластері k3d. На вже встановленому кластері
```
kubectl create namespace argocd
wget https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
nano install.yaml 
kubectl apply -n argocd -f install.yaml
kubectl port-forward svc/argocd-server -n argocd 8080:443&
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```
![Image](/doc/data/argocd-init.gif)

Після port-forward додатково налаштовано nginx proxy на тому ж сервері. Приклад конфігурації для nginx лежить в репозиторії. Після отримання паролю в консолі можемо зайти з ним через веб-браузер за посиланням http://{server ip address}:8090

Далі створюємо за демо деплой аплікейшена:

![Image](/doc/data/argocd-login.gif)

 https://github.com/den-vasyliev/go-demo-app

 Після перевірки запуску всіх подів можна створити port-forward та перевірити що додаток працює
 ```
  kubectl get svc -n demo
   
  kubectl port-forward -n demo svc/ambassador 8081:80&
  curl -F 'image=@./data/totoro.png' localhost:8081/img/
 ```

 ![Image](/doc/data/app-test.gif)

Додаток працює.

