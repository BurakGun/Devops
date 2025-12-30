# DevOps & Kubernetes Fundamentals

Welcome to the comprehensive guide on modern deployment strategies and Kubernetes architecture.

## Table of Contents
- [1. Evolution of Deployment](#1-evolution-of-deployment)
- [2. Kubernetes (K8s) Orchestration](#2-kubernetes-k8s-orchestration)
- [3. Core Architecture](#3-core-architecture)
- [4. Master Node Components (Control Plane)](#4-master-node-components-control-plane)
- [5. Worker Node Components](#5-worker-node-components)
- [6. Workload Management](#6-workload-management)
- [7. Services & Networking](#7-services--networking)
- [8. Ingress](#8-ingress)
- [9. Advanced Networking: Egress & CNI](#9-advanced-networking-egress--cni)
- [10. Summary of Traffic Flow & CNI Metrics](#10-summary-of-traffic-flow--cni-metrics)
- [11. Storage Management](#11-storage-management)
- [12. Container Storage Interface (CSI)](#12-container-storage-interface-csi)
- [13. Configuration & Secrets](#13-configuration--secrets)
- [14. Deployment Methodologies](#14-deployment-methodologies)
- [15. Pod Lifecycle & Health Checks](#15-pod-lifecycle--health-checks)
- [16. Resource Management (Requests & Limits)](#16-resource-management-requests--limits)
- [17. Security: RBAC (Role-Based Access Control)](#17-security-rbac-role-based-access-control)
- [18. Kubectl Cheat Sheet](#18-kubectl-cheat-sheet)
---

## 1. Evolution of Deployment

### Traditional Deployment
Manual and server-based method of installing software directly on an operating system or hardware.

### Virtualization
The division of physical resources into logical units through software.

### Docker Containers
Containers share the host OS kernel, making them lightweight and efficient.

<img width="673" alt="Traditional vs Virtualization vs Containerization" src="https://github.com/user-attachments/assets/9dd2052c-e9f0-4a43-ac53-63a727b0bbbe" />

---

## 2. Kubernetes (K8s) Orchestration

Kubernetes is an open-source platform for automating deployment and management of containerized applications.

### Isolation Mechanisms
1. **Namespace:** A virtual space to logically group resources.
   - `kube-system`: Core system objects.
   - `kube-public`: Globally accessible resources.
   - `kube-node-lease`: Node heartbeats.
2. **Control Group (cgroups):** Ensures resource limitation (CPU, Memory).

<img width="645" alt="Kubernetes Isolation" src="https://github.com/user-attachments/assets/9920aa9a-e72e-4043-8a5b-ffb4bbf545a7" />

---

## 3. Core Architecture
K8s architecture consists of two main node types:
- **Master Node:** Manages the cluster state.
- **Worker Node:** Runs the actual applications (Pods).

<img width="672" alt="Kubernetes Cluster Architecture" src="https://github.com/user-attachments/assets/6a2db1f1-72bf-4941-b110-7efbd71ea1bf" />

---

## 4. Master Node Components (Control Plane)

| Component | Description |
| :--- | :--- |
| **Etcd** | Consistent key-value store for all cluster data. |
| **API Server** | Central management hub and entry point. |
| **Controller Manager** | Regulates cluster state. |
| **Scheduler** | Assigns Pods to Nodes. |

---

## 5. Worker Node Components

- **Kubelet:** Primary agent ensuring containers are running in a Pod.
- **Kube-proxy:** Maintains network rules on nodes.
- **Container Runtime:** Software responsible for running containers (e.g., containerd).

---

## 6. Workload Management

### Desired vs. Actual State
Kubernetes constantly works to match the **Actual State** to the **Desired State**.

<img width="588" alt="Reconciliation Loop" src="https://github.com/user-attachments/assets/b400e7c4-8229-4ba0-b496-b40d818b767a" />

### Controller Types
- **Deployments:** Best for stateless applications.
- **StatefulSets:** For applications requiring stable identifiers (Databases).
- **DaemonSets:** Runs a pod copy on every node.
- **Jobs/CronJobs:** For batch or scheduled tasks.

---

## 7. Services & Networking

### 📋 Kubernetes Service Types Comparison Table

| Feature | ClusterIP (Default) | NodePort | LoadBalancer | ExternalName |
| :--- | :--- | :--- | :--- | :--- |
| **Access Scope** | 🔒 Internal only | 🌍 External + Internal | ☁️ External + Internal | ↗️ Internal to External |
| **IP Address** | Single Virtual Cluster IP | Node IP + Static Port | External (Public) IP | DNS Name (CNAME) |
| **Port Range** | Any port | 30000 - 32767 | Any port | N/A |
| **Primary Use Case** | Inter-service communication | Testing/Debug access | Production workloads | Alias for external services |
| **Cloud Integration** | Not required | Not required | **Required** (AWS, GCP, etc.) | Not required |

---

#### 💡 Quick Summary for Selection:
* **Use ClusterIP** for internal communication between your microservices (e.g., Backend to DB).
* **Use NodePort** for temporary exposure or when a cloud load balancer is not available.
* **Use LoadBalancer** for production-grade applications that need to be reachable from the internet.
* **Use ExternalName** to create an alias for a service that exists outside the cluster (e.g., an external RDS instance).

#### Visualizing Network Flows:

**NodePort:**

<img width="491" src="https://github.com/user-attachments/assets/853df695-1c44-429f-a6c8-7dc8e458b28e" />

**ClusterIP:**

<img width="552" src="https://github.com/user-attachments/assets/f9896c2e-f2c7-40bb-a81d-f400e696cae6" />

**LoadBalancer:**

<img width="505" src="https://github.com/user-attachments/assets/5571f223-346d-4725-b735-b0c6d9f90500" />

---

## 8. Ingress

**Ingress** routes external HTTP/HTTPS traffic using host or path-based rules.

### Path Matching Types
| Path Type | Description |
| :--- | :--- |
| **Exact** | Exact path match. |
| **Prefix** | Matches based on URL prefix. |

<img width="781" src="https://github.com/user-attachments/assets/f3763785-a731-4420-a270-faff936c29d2" />

---

## 9. Advanced Networking: Egress & CNI

### Egress
Traffic flowing **outwards** from a Pod to external destinations (APIs, DBs).

### CNI (Container Network Interface)
Standard for configuring network interfaces in containers.

---

## 10. Summary of Traffic Flow & CNI Metrics

* **Ingress:** Traffic **IN**.
* **Egress:** Traffic **OUT**.
* **CNI:** **INTERNAL** communication.

### 📋 CNI Driver Metrics
| Provider | Stars ⭐ | Forks 🍴 | Contributors 👥 |
| :--- | :---: | :---: | :---: |
| **Cilium** | 22.7k | 3.4k | 1,002 |
| **Flannel** | 9.3k | 2.9k | 243 |
| **Calico** | 6.8k | 1.5k | 387 |

<img width="775" src="https://github.com/user-attachments/assets/0d5d9857-e30a-4b39-9011-985a4aaa2c7c" />

---

## 11. Storage Management

### Persistent vs. Ephemeral Comparison
| Feature | Persistent (PV) | Ephemeral |
| :--- | :--- | :--- |
| **Retention** | Permanent | Temporary |

Volumes: Kubernetes volumes provide a way for containers in a pod to access and share data via the filesystem.
Persistent Volumes: It refers to a portion of the storage space allocated for use. It is a permanent form of storage.
Ephemeral Volumes: It refers to a portion of the storage space allocated for use. It is a non-persistent form of storage. For example, cache, RAM. 

<img width="774" src="https://github.com/user-attachments/assets/cd16b916-e08a-4842-b619-c7d6bb941aeb" />

| Type | purpose of use |
| :--- | :--- |
| **EmptyDir:** | Temporary disk space. Used for purposes such as caching. |
| **HostPath:** | It is used to bind a file created on the node to the pod. It only works on pods on a single node. |
| **Persisten Volume(PV):** | It is a permanent storage area. |
| **Persistent Volume Claim(PVC):** | It is the connection of the storage area to the pod. |
| **PersistentVolumeReclaimPolicy:** | After the pods are deleted, it shows what will happen to the data. Retain keeps the data. Delete removes it. |

### PersistentVolume Reclaim Policy
| Policy | Action |
| :--- | :--- |
| **Retain** | Keep data manually. |
| **Delete** | Remove volume automatically. |
| **Recycle** | Basic scrub (`rm -rf`). |

Static Binding = PV + PVC

Dynamic Binding = Storage Class + Storage Provisioner

<img width="786" src="https://github.com/user-attachments/assets/f8ab5c65-97f1-403a-82fd-db9342c855f9" />

---

## 12. Container Storage Interface (CSI)

CSI is a standard enabling K8s to integrate with 150+ third-party storage solutions.

<img width="780" src="https://github.com/user-attachments/assets/835e679a-d7a1-4d93-8260-0d9b7da901ad" />

---

## 13. Configuration & Secrets

### ConfigMap
Non-confidential data in key-value pairs.

<img width="712" src="https://github.com/user-attachments/assets/348dd2bb-125e-4b9a-82f0-e0022512d453" />

### Secret
Sensitive data (passwords, keys).

<img width="677" src="https://github.com/user-attachments/assets/1d501522-b20d-4401-b3fa-f7e9ffba4fb2" />

---

## 14. Deployment Methodologies

### Imperative vs. Declarative
- **Imperative:** Manual steps (`kubectl run`).
- **Declarative:** YAML file defined state (**Best Practice**).

**Imperative:**

<img width="172" src="https://github.com/user-attachments/assets/bf5e4e96-5e64-425a-86cc-ae880d1e52b3" />

**Declarative:**

<img width="241" src="https://github.com/user-attachments/assets/8e1e91ae-45fa-4f13-b8a8-0035a8909c4d" />

---
## 15. Pod Lifecycle & Health Checks

| Status | Description |
| :--- | :--- |
| **Pending** | Pod accepted, but containers not yet running. |
| **Running** | Containers created and at least one is running. |
| **Failed** | At least one container exited with error. |

* **Liveness Probe:** Checks if the app is alive (restarts if fails).
* **Readiness Probe:** Checks if ready to serve traffic.

---

## 16. Resource Management (Requests & Limits)

* **Requests:** Minimum resources needed to start.
* **Limits:** Maximum resources allowed to consume.

---

## 17. Security: RBAC (Role-Based Access Control)

1. **Role:** Permissions within a **Namespace**.
2. **ClusterRole:** Permissions across the **entire Cluster**.
3. **RoleBinding:** Connects user to a Role.

---

## 18. Kubectl Cheat Sheet

| Action | Command |
| :--- | :--- |
| **Status** | `kubectl get pods`, `kubectl get nodes` |
| **Debug** | `kubectl describe pod <name>`, `kubectl logs <name>` |
| **Apply** | `kubectl apply -f <file.yaml>` |
| **Interactive** | `kubectl exec -it <pod-name> -- /bin/bash` |

---
