# 🚀 Task 05 - Kubernetes Rolling Updates & Rollbacks

## 📌 Objective

Learn how Kubernetes performs rolling updates with a Deployment, monitor the rollout process in real time, and safely roll back to a previous application version.

---

## 🎯 Learning Outcomes

By the end of this lab, I can:

* Create a Deployment with multiple replicas.
* Understand why image versions should be explicitly specified.
* Perform a rolling update using a new container image.
* Monitor a rollout using Kubernetes commands.
* Understand the relationship between Deployments, ReplicaSets, and Pods.
* Roll back to a previous Deployment revision.
* Verify Deployment history and current image version.

---

## 🏗️ Architecture

```text
Deployment
     │
     ▼
ReplicaSet
     │
 ┌───┼───┐
 ▼   ▼   ▼
Pod Pod Pod
```

During a rolling update:

```text
Deployment
        │
        ├───────────────┐
        ▼               ▼
Old ReplicaSet    New ReplicaSet
   (1.26)            (1.25)
        │               │
   Old Pods        New Pods
```

Kubernetes gradually creates new Pods before removing old ones, minimizing downtime.

---

## 📂 Deployment Configuration

* Deployment Name: `nginx-rolling`
* Replicas: `3`
* Initial Image: `nginx:1.26`
* Updated Image: `nginx:1.25`

---

## 🛠️ Commands Used

### Apply Deployment

```bash
kubectl apply -f deployment.yaml
```

### Watch Rollout Status

```bash
kubectl rollout status deployment/nginx-rolling
```

### Watch ReplicaSets

```bash
kubectl get rs -w
```

### Watch Pods

```bash
kubectl get pods -w
```

### Update Image

```bash
kubectl set image deployment/nginx-rolling nginx=nginx:1.25
```

### Verify Current Image

```bash
kubectl get deployment nginx-rolling -o=jsonpath='{.spec.template.spec.containers[0].image}'
echo
```

### View Rollout History

```bash
kubectl rollout history deployment/nginx-rolling
```

### Roll Back

```bash
kubectl rollout undo deployment/nginx-rolling
```

---

## 🔍 Observations

* Kubernetes **does not modify existing Pods**.
* A **new ReplicaSet** is created whenever the Pod template changes.
* New Pods are created using the updated image.
* Old Pods are terminated only after the new Pods become Ready.
* The Deployment object remains the same while managing different ReplicaSets.
* Services continue routing traffic to healthy Pods throughout the update process.

---

## ⚠️ Issues Encountered

### 1. Image Version Not Displayed

**Issue**

The Deployment showed:

```text
nginx
```

instead of:

```text
nginx:1.26
```

**Cause**

The Deployment was created without specifying an image tag, causing Kubernetes to use the default `latest` tag.

**Solution**

Use explicit image versions.

```yaml
image: nginx:1.26
```

---

### 2. No YAML File Found

**Issue**

Unable to locate the Deployment YAML.

**Cause**

The Deployment was created directly using the `kubectl create deployment` command instead of a YAML manifest.

**Solution**

Generate a clean YAML manifest.

```bash
kubectl create deployment nginx-rolling \
  --image=nginx:1.26 \
  --dry-run=client \
  -o yaml > deployment.yaml
```

---

## 💡 Key Learnings

* A Deployment manages ReplicaSets.
* ReplicaSets manage Pods.
* Pods are immutable; changing the image requires creating new Pods.
* Rolling updates replace Pods gradually to reduce downtime.
* ReplicaSets keep revision history, allowing rollbacks.
* Production environments should always use versioned container images instead of `latest`.
* Kubernetes automatically maintains the desired number of replicas during updates.

---

## 📚 Kubernetes Objects Used

* Deployment
* ReplicaSet
* Pod

---

## ✅ Lab Completed

Successfully completed:

* Deployment creation
* Multi-replica Deployment
* Rolling Update
* Live rollout monitoring
* ReplicaSet observation
* Pod lifecycle observation
* Rollback to previous revision
* Image verification

---

## 📖 Next Task

**Task 06 – ConfigMaps**

Learn how to externalize application configuration using Kubernetes ConfigMaps and consume configuration through environment variables and mounted volumes.
