# K8s - minikube - kubectl commands

## minikube

### Requirements

For being able to interact with your minikube cluster [kubectl](#kubectl) is required,

### Installation

Docs to [install](https://minikube.sigs.k8s.io/docs/start/?arch=%2Flinux%2Fx86-64%2Fstable%2Fdebian+package#Service) it and get started!

### Common commands

```bash
minikube start
# start a local cluster for dev porpuses

minikube tunnel
# creates a tunnel to our local machine to enable functionality for service objects.
# it keeps alive until you stop execution usint Ctl+C

```

## kubectl

Docs to [install](https://kubernetes.io/docs/tasks/tools/#kubectl) it.

```bash
kubectl cluster-info
# start a local cluster for dev porpuses

kubectl get nodes
# return the list of nodes that compose the cluster

kubectl get namespaces
# helps to isolate applications

kubectl get pods -A
# list the pods from all existing namespaces

kubectl get services -A
# services works like LB and redirect traffic to pods

kubectl apply -f namespaces.yml
# helps you to create or modify namespaces on your cluste

kubectl delete -f namespaces.yml
# helps you to remove namespaces on your cluste

kubectl apply -f deployment.yml
# used to deploy a service in the cluster

kubectl get deployments -n __NAMESPACE__

kubectl delete pod POD_NAME -n NAMESPACE

kubectl exec -it __POD_NAME__ -- /bin/sh
# start an interactive session to a pod

kubectl logs __POD_NAME__ -n __NAMESPACE__

```
