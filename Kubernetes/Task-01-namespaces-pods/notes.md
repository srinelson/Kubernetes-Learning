# Task: Setup Kubernetes Namespaces and Pods

## 📌 Overview

This lab covers the fundamental Kubernetes concepts required to start working with a cluster using `kubectl` and YAML manifests.

## 🚀 Commands Practiced

```bash
kubectl create namespace dev
kubectl apply -f lab.yaml
kubectl get ns
kubectl get pods -n dev
kubectl describe ns dev
kubectl delete pod <pod-name> -n dev
kubectl delete namespace dev
```
## Learnings
- Pods live inside a namespace; without -n it defaults to `default`
- A single YAML file can contain multiple resources separated by `---`.
- `kubectl apply` only updates resources when changes are detected.
- Pods can be deployed directly into a Namespace using the `namespace` field.
- `vi` is the preferred editor for Kubernetes administration and CKA exam environments.

## 🎯 Goal

Build a strong foundation in Kubernetes by mastering the core resources before moving on to Deployments, ReplicaSets, Services, and Networking.
