# Task 02 - Set Resource Limits in Kubernetes Pods

## Objective

Create a Kubernetes Pod running an Apache HTTP Server container and configure container-level resource requests and limits to control CPU and memory utilization.

## Requirements

Create:

- Pod Name: `httpd-pod`
- Container Name: `httpd-container`
- Image: `httpd:latest`

### Resource Requests

| Resource | Value |
|-----------|---------|
| Memory | 15Mi |
| CPU | 100m |

### Resource Limits

| Resource | Value |
|-----------|---------|
| Memory | 20Mi |
| CPU | 100m |

---

## Pod Manifest

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: httpd-pod
spec:
  containers:
    - name: httpd-container
      image: httpd:latest
      resources:
        requests:
          memory: "15Mi"
          cpu: "100m"
        limits:
          memory: "20Mi"
          cpu: "100m"
```

## Deploy the Pod

```bash
kubectl apply -f pod.yaml
```

Expected Output:

```bash
pod/httpd-pod created
```

---

## Verify Deployment

Check Pod Status:

```bash
kubectl get pods
```

Example:

```bash
NAME        READY   STATUS    RESTARTS   AGE
httpd-pod   1/1     Running   0          11s
```

Inspect Resource Configuration:

```bash
kubectl describe pod httpd-pod
```

Look for:

```text
Limits:
  cpu:     100m
  memory:  20Mi

Requests:
  cpu:      100m
  memory:   15Mi
```

---

## Troubleshooting

While creating the pod, the following error was encountered:

```text
quantities must match the regular expression
```

### Cause

Resource values were not specified using valid Kubernetes quantity formats.

### Fix

Use valid units and quote memory values:

```yaml
memory: "15Mi"
memory: "20Mi"
cpu: "100m"
```

After correcting the resource definitions, the pod was successfully deployed.

---

## Learning Outcomes

- Understanding Kubernetes resource requests and limits.
- Configuring CPU and memory constraints.
- Troubleshooting Kubernetes manifest validation errors.
- Verifying pod resource allocation using kubectl.

---

## Author

**Sri Nelson Palnerselvam**

Kubernetes Learning Journey 🚀
``
