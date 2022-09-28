# loadbalanced-minikube-cluster

Using locally running Kubernetes (minikube) we can create both a cluster and have a service act as a loadbalancer.

There are multiple ways of doing this from kubectl commands through to building form yaml files.

For the purpose of this tutorial we will use the cli (kubectl) as it offers the best way of showing what is happening.

#Prerequisites: Have up to date versions of Docker and Minikube running


```
minikube start
```
Start minikube

```
minikube dashboard
```
The dashboard should open in a browser tab

```
kubectl create deployment hello-node --image=registry.k8s.io/echoserver:1.4
```

```
kubectl get deployments
```
Now check the deployments status

```
kubectl get pods
```
You should jsut have 1 pod running at this stage.

```
get events
```
See the stages/process of the deployment

```
kubectl config view
```
A useful output which can be used if you wish to run a similar deployment from a config file.

```
kubectl expose deployment hello-node --type=LoadBalancer --port=8080
```
Expose port 8080 so we can start communicating
```
kubectl get services
```
View the ip addresses and ports of your deployment

```
minikube service hello-node
```
Returns the url for the service in your cluster

```
minikube addons enable metrics-server
```
Enable metrics if you wish to use them at a later date for troubleshooting

```
kubectl get pod,svc -n kube-system
```
View the pod and service you created

# Scaling the cluster

```
kubectl get deployments
```
Just so you can see where you are

```
kubectl get rs
```
View the replica set

```
kubectl scale deployments/hello-node --replicas=4
```
Set replicas to 4

```
kubectl get deployments 
```
Confirm you now have 4 instances of the application

```
kubectl get pods -o wide
```
Confirm you now have 4 pods

```
kubectl describe deployments/hello-node
```
Confirm you have 4 replicas now


# Setting up the loadbalancer

```
kubectl describe services/hello-node
```
Use the command to find out the exposed IP and Port

```
export NODE_PORT=$(kubectl get services/hello-node -o go-template='{{(index .spec.ports 0).nodePort}}')
```
```
echo NODE_PORT=$NODE_PORT
```
Create an env variable called NODE_PORT
# Testing the loadbalancer

Each pod has a different ip address and so if you curl the node port a number of times you will see different ip addresses in the response thus proving that the loadbalancing is working
```
curl $(minikube ip):$NODE_PORT
```


# Thank you for taking your time to complete this tutorial
