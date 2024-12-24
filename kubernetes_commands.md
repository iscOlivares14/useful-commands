# K8s - minikube - kubectl commands

## minikube

### Requirements

For being able to interact with your minikube cluster [kubectl](#kubectl) is required,

### Installation

Docs to [install](https://minikube.sigs.k8s.io/docs/start/?arch=%2Flinux%2Fx86-64%2Fstable%2Fdebian+package#Service) it and get started!

### Common minikube commands

```bash
minikube start
# start a local cluster for dev porpuses

minikube tunnel
# creates a tunnel to our local machine to enable functionality for service objects.
# it keeps alive until you stop execution usint Ctl+C

```

## kind

### Requirements

For being able to interact with your minikube cluster [kubectl](#kubectl) is required,

### Installation

Docs to [install](https://minikube.sigs.k8s.io/docs/start/?arch=%2Flinux%2Fx86-64%2Fstable%2Fdebian+package#Service) it and get started!

### Common kind commands

```bash
minikube start
# start a local cluster for dev porpuses

minikube tunnel
# creates a tunnel to our local machine to enable functionality for service objects.
# it keeps alive until you stop execution usint Ctl+C

```

## kubectl

Docs to [install](https://kubernetes.io/docs/tasks/tools/#kubectl) it.

### Common kubectl commands

```bash
kubectl cluster-info
# start a local cluster for dev porpuses

kubectl api-resources
# shows all existant resources in the cluster

kubectl get pods -n kube-system
# shows all resources used by k8s to work

kubectl get nodes
# return the list of nodes that compose the cluster

kubectl get namespaces
# helps to isolate applications

kubectl get pods -A
# list the pods from all existing namespaces

kubectl get services -A
# services works like LB and redirect traffic to pods

kubectl apply -f __YML_FILE__.yaml
# helps you to create or modify workloads on your cluste

kubectl delete -f __YML_FILE__.yaml
# helps you to remove workloads on your cluster

kubectl get deployments -n __NAMESPACE__

kubectl delete pod POD_NAME -n NAMESPACE

kubectl exec -it __POD_NAME__ -- /bin/sh
# start an interactive session to a pod

kubectl logs __POD_NAME__ -n __NAMESPACE__

kubectl logs __ETCD_POD_name__ -n kubesystem | jq .
# shows latest activity on the cluster

```

### Create worload files

```bash
kubectl create deployment --dry-run=[client|server] --image <image_full_path> <deployment_name> --output=yaml > <file_to_persist_in>

kubectl create deployment --dry-run=client --image localhost:5000/explorecalifornia.com explorecalifornia.com --output=yaml
```