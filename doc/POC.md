#POC Guide

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

![Image]

Наступним етапом буде розгортання системи ArgoCD в кластері k3d. На вже встановленому кластері
