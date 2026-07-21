# 🚀 Task 03 - Deploying NGINX on a 3-Node Kind Cluster

## 📖 Overview

This lab demonstrates how to:

- Create a **3-node Kubernetes cluster** using **Kind**
- Deploy an **NGINX** application
- Expose the application using a **Service**
- Access the application locally
- Troubleshoot common deployment issues such as **ImagePullBackOff**

This exercise is excellent practice for the **Certified Kubernetes Administrator (CKA)** exam.

---

# 🎯 Learning Outcomes

By completing this lab, you will be able to:

- ✅ Create a multi-node Kind cluster
- ✅ Verify Kubernetes nodes
- ✅ Deploy applications using a Deployment
- ✅ Expose applications using a Service
- ✅ Understand the difference between ClusterIP and NodePort
- ✅ Troubleshoot ImagePullBackOff errors
- ✅ Practice essential `kubectl` commands used in the CKA exam

---

# 📦 Prerequisites

Ensure the following tools are installed:

- Docker Desktop
- WSL Ubuntu
- kubectl
- Kind

Verify your installation:

```bash
docker version
kubectl version --client
kind version
```

---

# 🏗️ Step 1 - Create a 3-Node Kind Cluster

Create a configuration file named:

```text
kind-config.yaml
```

Contents:

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4

nodes:
  - role: control-plane
  - role: worker
  - role: worker
```

Create the cluster:

```bash
kind create cluster --name lab --config kind-config.yaml
```

Verify the cluster:

```bash
kubectl get nodes
kubectl get nodes -o wide
```

Expected output:

```
lab-control-plane
lab-worker
lab-worker2
```

---

# 🚀 Step 2 - Deploy NGINX

Create the deployment:

```bash
kubectl create deployment nginx --image=nginx
```

Verify:

```bash
kubectl get deployment
kubectl get pods
kubectl get pods -o wide
```

Describe the deployment:

```bash
kubectl describe deployment nginx
```

---

# 🌐 Step 3 - Expose the Deployment

Create a **NodePort** Service:

```bash
kubectl expose deployment nginx \
  --type=NodePort \
  --port=80 \
  --target-port=80
```

Verify:

```bash
kubectl get svc
kubectl describe svc nginx
```

---

# 🌍 Step 4 - Access the Application

Forward the service locally:

```bash
kubectl port-forward service/nginx 8080:80
```

Open your browser:

```
http://localhost:8080
```

You should see the default **Welcome to NGINX!** page.

---

# 🔍 Useful kubectl Commands

## Nodes

```bash
kubectl get nodes
kubectl get nodes -o wide
kubectl describe node <node-name>
```

---

## Pods

```bash
kubectl get pods
kubectl get pods -o wide
kubectl get pods -A

kubectl describe pod <pod-name>

kubectl delete pod <pod-name>
```

---

## Logs

Default namespace:

```bash
kubectl logs <pod-name>
```

Specific namespace:

```bash
kubectl logs -n <namespace> <pod-name>
```

Follow logs:

```bash
kubectl logs -f <pod-name>
```

Previous container logs:

```bash
kubectl logs -p <pod-name>
```

---

## Services

```bash
kubectl get svc

kubectl describe svc nginx

kubectl delete svc nginx
```

---

# 🧠 Important kubectl Flags

| Flag | Description |
|------|-------------|
| `-A` | All namespaces |
| `-n` | Namespace |
| `-o` | Output format |
| `-f` | Apply YAML file |
| `-l` | Label selector |
| `-w` | Watch resources |

Common output formats:

```bash
-o wide
-o yaml
-o json
-o name
```

---

# ⚠️ Troubleshooting

## ImagePullBackOff

Describe the Pod:

```bash
kubectl describe pod <pod-name>
```

Common causes:

- Docker Hub unavailable
- Network connectivity issues
- Corporate proxy
- TLS certificate issues
- `x509: certificate signed by unknown authority`

Possible reasons:

- Corporate SSL inspection
- Proxy configuration
- Missing trusted CA inside Kind/containerd

Verify the image can be downloaded:

```bash
docker pull nginx
```

---

## Service Already Exists

Error:

```
services "nginx" already exists
```

Check existing services:

```bash
kubectl get svc
```

Delete the existing service:

```bash
kubectl delete svc nginx
```

Recreate it:

```bash
kubectl expose deployment nginx \
  --type=NodePort \
  --port=80 \
  --target-port=80
```

---

# 🎓 CKA Exam Tips

- Memorize common `kubectl` commands.
- Use `kubectl explain` to understand manifest fields.
- Generate YAML quickly:

```bash
kubectl create deployment nginx \
  --image=nginx \
  --dry-run=client -o yaml
```

Always verify using:

```bash
kubectl get
kubectl describe
kubectl logs
```

Troubleshooting workflow:

```
Pod
   ↓
Events
   ↓
Logs
   ↓
Service
   ↓
Endpoints
```

---

# ✅ Lab Checklist

- [x] Created a 3-node Kind cluster
- [x] Verified all nodes are Ready
- [x] Created an NGINX Deployment
- [x] Verified the Pod is Running
- [x] Exposed the Deployment using a Service
- [x] Accessed the application successfully
- [x] Practiced troubleshooting ImagePullBackOff
- [x] Documented commands and observations

---

# 💡 Key Takeaways

- Kind creates Kubernetes nodes as Docker containers.
- Nodes are cluster-scoped and are **not** part of namespaces.
- Pods run inside namespaces.
- Deployments manage Pods.
- Services expose Deployments to other applications or users.
- `kubectl describe pod` is the first command to investigate `ImagePullBackOff`.
- The most valuable troubleshooting commands are:

```bash
kubectl get
kubectl describe
kubectl logs
```

---

## 📚 Skills Practiced

- Kubernetes
- Kind
- Deployments
- Pods
- Services
- NodePort
- Port Forwarding
- kubectl
- Troubleshooting
- CKA Fundamentals
