# loadbalanced-minikube-cluster

Using locally running Kubernetes (minikube) we can create both a cluster and have a service act as a loadbalancer.

There are multiple ways of doing this from kubectl commands through to building form yaml files.

For the purpose of this tutorial we will use the cli (kubectl) as it offers the best way of showing what is happening.

#Prerequisites: Have up to date versions of Docker and Minikube running


```
minikube start
```

```
minikube dashboard
```

```
kubectl create deployment hello-node --image=registry.k8s.io/echoserver:1.4
```

```
kubectl get deployments
```

```
kubectl get pods
```

```
get events
```

```
kubectl config view
```

```
kubectl expose deployment hello-node --type=LoadBalancer --port=8080
```

```
kubectl get services
```

```
minikube service hello-node
```

```
minikube addons enable metrics-server
```

```
kubectl get pod,svc -n kube-system
```

```
kubectl get deployments
```

```
kubectl get rs
```

```
kubectl scale deployments/hello-node --replicas=4
```

```
kubectl get deployments 
```

```
kubectl get pods -o wide
```

```
kubectl describe deployments/hello-node
```

```
kubectl describe services/hello-node
```

```
export NODE_PORT=$(kubectl get services/hello-node -o go-template='{{(index .spec.ports 0).nodePort}}')
```

```
echo NODE_PORT=$NODE_PORT
```

# Testing the loadbalancer

Each pod has a different ip address and so if you curl the node port a number of times you will see different ip addresses in the response thus proving that the loadbalancing is working
```
curl $(minikube ip):$NODE_PORT
```


# Thank you for taking your time to complete this tutorial
