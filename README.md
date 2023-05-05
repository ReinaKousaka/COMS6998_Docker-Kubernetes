# COMS6998_Docker-Kubernetes

### Overview

This is the Assignment 3 for course COMS6998: Cloud Computing and Big Data at Columbia University.  <br>
The task is to deploy a web application on Kubernetes using Docker containers. <br>
Details can be found in this [PDF](/Assignment%203.pdf)

### Workflow

Commands

```bash
# Part 2
docker build -t sy3079/flask-app:latest .
docker-compose up
docker push sy3079/flask-app:latest

# Part 3
minikube start
kubectl apply -f app-depolyment.yaml
minikube service flask-app-service --url

# Part 5
kubectl get pods
kubectl delete pod ${podName}
# should genereate a new pod
kubectl get pods

# Part 8
# Setup Prometheus monitoring on Kubernetes
# reference: https://devopscube.com/setup-prometheus-monitoring-on-kubernetes/
kubectl create namespace monitoring
kubectl apply -f clusterRole.yaml
kubectl apply -f config-map.yaml
kubectl apply -f prometheus-deployment.yaml
kubectl get deployments --namespace=monitoring
kubectl get pods --namespace=monitoring
kubectl port-forward ${podName} 8080:9090 -n monitoring
# Setup Kube state metrics on Kubernetes
# reference: https://devopscube.com/setup-kube-state-metrics/
git clone https://github.com/devopscube/kube-state-metrics-configs.git
kubectl apply -f kube-state-metrics-configs/
kubectl get deployments kube-state-metrics -n kube-system
# Setup alert manager on Kubernetes
# reference: https://devopscube.com/alert-manager-kubernetes-guide/#
git clone https://github.com/bibinwilson/kubernetes-alert-manager.git
kubectl apply -f kubernetes-alert-manager/
kubectl apply -f cpu_stress.yaml
# kubectl apply -f config-map.yaml

# test alert manager
kubectl get pods -n monitoring -l app=prometheus-server -o name | xargs kubectl delete -n monitoring
minikube service alertmanager --url -n monitoring

# go to the prometheus dashboard
kubectl -n monitoring port-forward svc/prometheus-service 1234:1234

# Part 4
# may create an EKS cluster using eksctl: https://github.com/weaveworks/eksctl
eksctl create cluster --name=cs6998 --nodes-min=3 --nodes-max=5

aws eks update-kubeconfig --region us-east-2 --name ${ClusterName}
kubectl apply -f app-depolyment.yaml
kubectl get svc flask-app-service
kubectl port-forward service/flask-app-service 5000:5000

# in case failed to depoly mongo due to aws pv/pvc
kubectl delete pvc mongo-pvc
kubectl apply -f pvc.yaml
kubectl apply -f pv.yaml
```

Some other helpful commands

```bash
# remove all docker images and containers
docker rm $(docker ps -aq)
docker image prune -fa
docker volume prune -f

kubectl get services
minikube dashboard

# list all contexts
kubectl config get-contexts
# display current context
kubectl config current-context
# switch kubectl context
kubectl config use-context ${contextName}
# list services
kubectl get svc
# access the app
kubectl port-forward service/<your-service-name> <local-port>:<service-port> --namespace <your-namespace>
```

### Team Members

- Songheng Yin, sy3079
- Yiming Zhu, yz4336
