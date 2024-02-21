# Stuff learnt after Googling

## Add Markdown Linter

⌘P -> `ext install markdownlint`

## Add Terraform for Kubernetes

<https://medium.com/rahasak/terraform-kubernetes-integration-with-minikube-334c43151931>

## Docker or Hyperkit

"The docker engine which is the core software behind the docker only runs on Linux kernal(the engine can run on a physical or a virtual machine, but it can only run on top of a Linux kernel i.e. any OS that is flavour of Linux). [...] Docker Desktop is a closed-source software that allows developers working on Windows/macOS to use container technology seamlessly on their development environment without needing to manage the complexity of operating a VM and all the dependencies that comes along with it (networking, virtualization, knowledge of linux etc)." from <https://medium.com/rahasak/replace-docker-desktop-with-minikube-and-hyperkit-on-macos-783ce4fb39e3>

## What are Load Balancers ?

Meant to efficiently distribute incoming network traffic across multiple servers or computing resources.
Load balancers can be implemented at various layers of the network stack, including application layer (Layer 7), transport layer (Layer 4), and network layer (Layer 3).

In environments without a cloud provider, Kubernetes simulates the behavior of a load balancer by using virtual IP addresses (ClusterIPs) and alternative methods like NodePort or port forwarding to expose Services to external traffic.

When you use Minikube to run Kubernetes locally, it sets up a single-node Kubernetes cluster within a virtual machine (VM) on your local system. This VM is assigned an IP address that can be used to access services running within the cluster.
