## Devops

# Traditional Deployment

Traditional deployment is a manual and often server-based method of installing, configuring, and updating software by installing it directly on an operating system, hardware, or server.

# Virtualization 

The division of physical resources into logical units through software.

# Docker Container 

Containers share the host operating system's kernel, making them more lightweight, faster to start, and efficient with resources. This technology streamlines the development, testing, and deployment process for modern applications, especially those using microservices.

<img width="673" height="384" alt="image" src="https://github.com/user-attachments/assets/9dd2052c-e9f0-4a43-ac53-63a727b0bbbe" />

## Kubernetes 

Kubernetes is an open-source orchestration platform used to deploy, scale, and manage containerised applications anywhere.(K8s)
Two technologies have been developed to separate the applications from each other. They are consist of Namespace and control group.

NameSpace: A virtual space that logically groups and isolates resources within a Kubernetes cluster. Each namespace must be unique name .

Default Namespaces;
Kube-system: Hosts the objects of the Kubernetes system.
Kube-Public: It is available for access to all namespaces.
Kube-node-lease: Used for node heartbeat operations.
 
Control Group: It ensures the limitation of physical resources that applications will use.

<img width="645" height="323" alt="image" src="https://github.com/user-attachments/assets/9920aa9a-e72e-4043-8a5b-ffb4bbf545a7" />

In a distributed computing architecture such as Kubernetes, They are consists of master and worker nodes.

Master Node: The responsibility of the master node is to manage the worker nodes.
Worker Node:  The responsibility of the worker node is to run the services.

<img width="672" height="322" alt="image" src="https://github.com/user-attachments/assets/6a2db1f1-72bf-4941-b110-7efbd71ea1bf" />

# Master Node Components

 ## Etcd :
It is a key-value database that stores the status and configurations of the control plane components of a Kubernetes cluster.

 ## Api Server :
The Kubernetes API is the most important component, serving as the communicate point to the cluster. It enables all components within the Kubernetes cluster to communicate with each other. Users or tools can directly access the Kubernetes API Server and create, update, or delete Kubernetes objects.

 ## Controller Manager : 
It is a background component that monitors the status of Kubernetes objects and intervenes when necessary. It consists of multiple control mechanisms, such as the Node controller, Job controller, and Endpoints controller, brought together under a single structure.

 ## Scheduler :
It is responsible for assigning newly created pods to an appropriate node and helps ensure the most efficient use of resources within the cluster.

# Worker Node Components

## Kube-proxy :
It is a network proxy running on a node in Kubernetes that manages the network rules within the cluster. Its primary tasks are to assign IP addresses to pods and to provide load balancing when multiple pods exist for a single service. Kube-proxy enables services to communicate with the outside world and with each other by directing requests to the correct pods.

## Kubelet :
Kubelet is the primary Kubernetes agent running on each node and responsible for managing pods . It listens for commands from the master node, creates and manages pods, monitors their status, and reports back to the cluster API server. This keeps the container runtime environment synchronised and facilitates communication between the master and the nodes.

## Container Runtime :
It is the software that creates, runs and manages the container.

 # Controller
Several controllers are bundled within the kube-controller-manager, each responsible for a specific aspect of cluster management:

 ## Node Controller:
Tracks the health and availability of Nodes in the cluster.
 
 ## Replication Controller:
Ensures the desired number of Pod replicas are running for Deployments or ReplicaSets.

 ## Endpoints Controller:
Maintains an Endpoints object for each Service, reflecting the current set of Pods that back that Service.

 ## Service Account Controller:
Creates and manages Service Accounts used for Pod authentication and authorization.

 ## Token Controller:
Responsible for issuing authentication tokens for service accounts.

 ## Namespace Controller:
Creates and manages Kubernetes namespaces, providing a way to isolate resources.

# Desired State and Actual State

 ## Desired state:
This is the configuration you define for your application and cluster. It's what you want to happen, like "run 5 replicas of my web server using this Docker image".

 ## Actual State:
This is the current, real-time state of the cluster, such as which pods are running, which nodes they are on, and their resource usage.

<img width="588" height="314" alt="image" src="https://github.com/user-attachments/assets/b400e7c4-8229-4ba0-b496-b40d818b767a" />

# Workload Management

 ## Deployments:
The process of taking a software application from the development stage to deployment and making it ready for use.

 ## ReplicaSets:
A Kubernetes object that ensures a specified number of Pods within a group are always running.

 ## StatefulSet:
It is used to manage applications such as databases that require persistent storage, stable unique network identifiers, and stateful information that necessitates sequential distribution and scaling.

 ## DaemonSet:
A DaemonSet is an object used to run a specific pod on each Node (server) in a Kubernetes cluster. This allows pods, which have a copy on each node in the cluster, to provide system-wide services. DaemonSets are typically used for log collection, monitoring, or other system services that need to run on every node.

 ## Job:
It is an assignment created to successfully complete a specific task. For example, when we create a new pod. We call the entire process a job.

 ## Cronjob:
CronJob is the execution of regularly scheduled tasks such as backups, report generation, etc.

 ## StatefulSet vs. DaemonSet vs. Deployment
While all three are pretty similar, and their main purpose is to create pods based on your configuration, they are used for the following:

StatefulSets are used for stateful applications, and they maintain a sticky identity for each of their pods.
DaemonSet are used to keep a copy of a pod on all the nodes inside the cluster, making them a great choice for node-level services.
Deployments manage stateless applications, providing declarative updates to applications with capabilities for scaling, rolling updates, and rollbacks.
 
# Services , Load Balancing and Networking 

 ## Service:
It enables you to access your application located in one or more pods within a cluster using a single IP address or DNS. This way, you can access the same service IP connection even if the pods change.

 ## NodePort
It assigns a specific port number to any node within the cluster and directs all traffic coming to the Service object through this port number to that node.

<img width="491" height="386" alt="image" src="https://github.com/user-attachments/assets/853df695-1c44-429f-a6c8-7dc8e458b28e" />

 ## ClusterIP
Pods only forward requests originating from resources within the Kubernetes cluster and possess a dedicated IP address within the cluster. This IP address directs requests intended for the relevant Pod group. It is solely used to enable communication between other resources within the Kubernetes cluster.

<img width="552" height="310" alt="image" src="https://github.com/user-attachments/assets/f9896c2e-f2c7-40bb-a81d-f400e696cae6" />

 ## LoadBalancer
A Service is exposed externally using a load balancer that forwards requests to the node's NodePort.Load balancers are not a Kubernetes component. 

<img width="505" height="397" alt="image" src="https://github.com/user-attachments/assets/5571f223-346d-4725-b735-b0c6d9f90500" />

## Comparison of Service Type: 

## Kubernetes Service Types Comparison

| Feature | ClusterIP (Default) | NodePort | LoadBalancer | ExternalName |
| :--- | :--- | :--- | :--- | :--- |
| **Access Scope** | Internal only | External + Internal | External + Internal | Internal to External |
| **IP Address** | Single Virtual Cluster IP | Node IP + Static Port | External (Public) IP | DNS Name (CNAME) |
| **Port Range** | Any port | 30000 - 32767 | Any port | N/A |
| **Primary Use Case** | Inter-service communication | Testing/Debug access | Production workloads | Alias for external services |
| **Cloud Integration** | Not required | Not required | Required (AWS, GCP, etc.) | Not required |

---

### Quick Summary of Types

1. **ClusterIP**: The default type. The service is only reachable from within the cluster.
2. **NodePort**: Opens a specific port on all Nodes. Great for quick testing when you don't have a Cloud Load Balancer.
3. **LoadBalancer**: The standard way to expose a service to the internet in a cloud environment (AWS, Azure, GCP).
4. **ExternalName**: Acts as a redirection to an external DNS name rather than routing traffic to pods.


