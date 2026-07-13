# Task: Setup Kubernetes Namespaces and Pods

## Commands used
kubectl create namespace dev-ns
kubectl run nginx-pod --image=nginx -n dev-ns
kubectl get pods -n dev-ns

## Learnings
- Pods live inside a namespace; without -n it defaults to `default`
