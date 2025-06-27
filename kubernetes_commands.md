# K8s - minikube - kubectl commands

## minikube

### Requirements

For being able to interact with your minikube cluster [kubectl](#kubectl) is required,

### Installation

Docs to [install](https://minikube.sigs.k8s.io/docs/start/?arch=%2Flinux%2Fx86-64%2Fstable%2Fdebian+package) it and get started!

### Common minikube commands

```bash
minikube start
# start a local cluster for dev porpuses

minikube stop
# stop local cluster

minikube dashboard --url

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

kubectl get namespaces
# helps to isolate applications

kubectl get pods -n kube-system
# shows all resources used by k8s to work

kubectl get nodes
# return the list of nodes that compose the cluster

kubectl describe nodes __NODENAME__
# this is for getting all description of a node. resources, event and so on.

kubectl get componentstatuses
# list k8s conponents and their status

kubectl get daemonsets -n kube-system kube-proxy
# kube-proxy is setup by node and its job is route the traffic to load balanced services in the cluster

kubectl get deployments -n kube-system (kube-dns|coredns)
kubectl get services -n kube-system kube-dns
# k8s runs a DNS service as a deployment its job is provide naming and discovery for services in the cluster.

kubectl get deployments -n kube-system kube-dashboard
# UI for k8s

kubectl get pods -A
# list the pods from all existing namespaces

kubectl describe pods
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

### Tutorial minikube

#### Create a deployment

```bash
kubectl create deployment kubernetes-bootcamp --image=gcr.io/k8s-minikube/kubernetes-bootcamp:v1

echo -e "Starting Proxy. After starting it will not output a response. Please return to your original terminal window\n"; kubectl proxy

curl http://localhost:8001/version

export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')
echo Name of the Pod: $POD_NAME

curl http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME

kubectl exec $POD_NAME -- env
kubectl exec -ti $POD_NAME -- bash 
```

#### Create a service

```bash
kubectl get services
kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080
kubectl get services
kubectl describe services/kubernetes-bootcamp

export NODE_PORT=$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}')
echo NODE_PORT=$NODE_PORT

curl $(minikube ip):$NODE_PORT
```

#### Play with labels
```bash
kubectl get pods -l app=kubernetes-bootcamp
kubectl get services -l app=kubernetes-bootcamp
export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')
echo Name of the Pod: $POD_NAME

kubectl label pods $POD_NAME version=v1
kubectl describe pods $POD_NAME
kubectl get pods -l version=v1
```

#### Delete a service
```bash
kubectl delete service -l app=kubernetes-bootcamp
kubectl get services
curl $(minikube ip):$NODE_PORT
kubectl exec -ti $POD_NAME -- curl localhost:8080
# service still runnng but not exposed out of the pod
```

#### Scale your deployment
```bash
kubectl get deployments
kubectl get rs
kubectl scale deployments/kubernetes-bootcamp --replicas=4
kubectl get pods -o wide
kubectl describe deployments/kubernetes-bootcamp

# to check load balancng you need to recreate the service as before then
kubectl describe services/kubernetes-bootcamp
export NODE_PORT=$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}')
echo NODE_PORT=$NODE_PORT
curl $(minikube ip):$NODE_PORT
# each request mut be replied by a different pod

# scale down
kubectl scale deployments/kubernetes-bootcamp --replicas=2
kubectl get deployments
kubectl get pods -o wide
```

### Create worload files

```bash
kubectl create deployment --dry-run=[client|server] --image <image_full_path> <deployment_name> --output=yaml > <file_to_persist_in>

kubectl create deployment --dry-run=client --image localhost:5000/explorecalifornia.com explorecalifornia.com --output=yaml
```