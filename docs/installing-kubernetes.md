# Installing Kubernetes Locally

Tutorial steps here: <https://kubernetes.io/docs/tutorials/hello-minikube/>

## Steps in order

Install some stuff:

```shell
brew install minikube
brew install hyperkit
brew install docker
brew install docker-compose
```

### Minikube start

```shell
minikube start
minikube dashboard  # This opens a web browser with the Kubernetes Dashboard
kubectl get nodes
```

### Deployment

```shell
# Creates a Deployment that manages a Pod. The Pod runs a Container based on the provided Docker image.
kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.39 -- /agnhost netexec --http-port=8080
kubectl get deployments
```

Check the Pod

```shell
kubectl get pods
# > NAME                         READY   STATUS    RESTARTS   AGE
# > hello-node-ccf4b9788-2f625   1/1     Running   0          9m59s

kubectl logs hello-node-ccf4b9788-2f625
# > I0221 08:34:40.722932       1 log.go:195] Started HTTP server on port 8080
# > I0221 08:34:40.723697       1 log.go:195] Started UDP server on port  8081
```

```shell
kubectl config view  # Returns a yaml with the cluster specs
```

### Create a service

```shell
# Exposes the deployment to outside of the cluster on port 8080
kubectl expose deployment hello-node --type=LoadBalancer --port=8080
```

In environments without a cloud provider, Kubernetes simulates the behavior of a load balancer by using virtual IP addresses (ClusterIPs) and alternative methods like NodePort or port forwarding to expose Services to external traffic. This allows to achieve similar functionality for accessing applications running in the Kubernetes cluster, but with some differences in setup compared to a cloud provider environment.

```shell
kubectl get services
# NAME         TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
# hello-node   LoadBalancer   10.98.247.230   <pending>     8080:31106/TCP   81s
# kubernetes   ClusterIP      10.96.0.1       <none>        443/TCP          37m
```

These IP addresses are assigned automatically by Kubernetes when you create Services within the cluster:

- `10.98.247.230`: Cluster IP address assigned to the `hello-node` Service. Cluster IP addresses are internal IP addresses used by Kubernetes for communication between different services within the cluster. They are typically not accessible from outside the cluster.
- `10.96.0.1`: Cluster IP address assigned to the `kubernetes` Service. This service is a special service provided by Kubernetes itself, representing the Kubernetes API server. The IP address 10.96.0.1 is a well-known IP address commonly used for the Kubernetes API server within the cluster.

On the Ports:

- `8080`: This is the port number that the Service is listening on internally within the Kubernetes cluster. When traffic reaches the Service within the cluster, it's directed to this port.
- `31106`: This is the NodePort assigned to the Service. NodePort is a port that is opened on all the nodes in the Kubernetes cluster, and traffic sent to this port is forwarded to the Service. So, externally, you can access the Service using any of the Kubernetes nodes' IP addresses along with this port.

```shell
minikube service hello-node
# > |-----------|------------|-------------|---------------------------|
# > | NAMESPACE |    NAME    | TARGET PORT |            URL            |
# > |-----------|------------|-------------|---------------------------|
# > | default   | hello-node |        8080 | http://192.168.64.2:31106 |
# > |-----------|------------|-------------|---------------------------|
```

When Minikube is used to run Kubernetes locally, it sets up a single-node Kubernetes cluster within a virtual machine (VM) on local. This VM is assigned an IP address that can be used to access services running within the cluster. In this case, the IP address of the Minikube VM is 192.168.64.2 and the NodePort is 31106.
