# COMS6998_Docker-Kubernetes

### Overview

This is the Assignment 3 for course COMS6998: Cloud Computing and Big Data at Columbia University.
The task is to deploy a web application on Kubernetes using Docker containers.

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
```

Some other helpful commands

```bash
# remove all docker images and containers
docker rm $(docker ps -aq)
docker image prune -fa
docker volume prune -f

kubectl get services
minikube dashboard
```

### Team Members

- Songheng Yin, sy3079
- Yiming Zhu, yz4336
