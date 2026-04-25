# Experiment 12: Container Orchestration using Kubernetes

---

## Objective
Study and analyse container orchestration using Kubernetes — deploying, scaling, and self-healing a WordPress application using Deployments and Services.

---

## Theory

### Why Kubernetes over Docker Swarm?

| Reason | Explanation |
|---|---|
| Industry standard | Most companies use Kubernetes in production |
| Powerful scheduling | Automatically decides where to run containers |
| Large ecosystem | Monitoring, logging, auto-scaling tools available |
| Cloud-native | Works on AWS, GCP, Azure natively |

### Core Concepts

| Docker Concept | Kubernetes Equivalent | Meaning |
|---|---|---|
| Container | Pod | Smallest unit — wraps one or more containers |
| Compose service | Deployment | Defines how app runs (image, replicas) |
| Load balancing | Service | Stable IP/port to access pods |
| Scaling | ReplicaSet | Ensures N pod copies always running |

---

## Environment

- **kubectl version:** v1.35.2
- **Cluster:** Docker Desktop Kubernetes (`docker-desktop`)
- **Cluster status:** Ready (control-plane)
- **No Minikube/k3d needed** — kubectl already connected to Docker Desktop's built-in cluster

```bash
kubectl get nodes
```
```
NAME             STATUS   ROLES           AGE   VERSION
docker-desktop   Ready    control-plane   30d   v1.34.1
```


---

## Task 1 — Create a Deployment

### wordpress-deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: wordpress
spec:
  replicas: 2
  selector:
    matchLabels:
      app: wordpress
  template:
    metadata:
      labels:
        app: wordpress
    spec:
      containers:
      - name: wordpress
        image: wordpress:latest
        ports:
        - containerPort: 80
```

### Apply and verify

```bash
kubectl apply -f wordpress-deployment.yaml
```
```
deployment.apps/wordpress created
```

```bash
kubectl get pods
```
```
NAME                         READY   STATUS    RESTARTS   AGE
wordpress-848c87c44f-h5542   1/1     Running   0          25s
wordpress-848c87c44f-n9kps   1/1     Running   0          25s
```

Both pods are `Running` with `READY 1/1`.

---

## Task 2 — Expose as a Service

### wordpress-service.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: wordpress-service
spec:
  type: NodePort
  selector:
    app: wordpress
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30007
```

### Apply and verify

```bash
kubectl apply -f wordpress-service.yaml
```
```
service/wordpress-service created
```

```bash
kubectl get svc
```
```
NAME                TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
kubernetes          ClusterIP   10.96.0.1       <none>        443/TCP        31d
wordpress-service   NodePort    10.99.82.207    <none>        80:30007/TCP   13s
```

---

## Task 3 — Verify and Access WordPress

```bash
kubectl get pods
```
```
NAME                         READY   STATUS    RESTARTS   AGE
wordpress-848c87c44f-h5542   1/1     Running   0          6m37s
wordpress-848c87c44f-n9kps   1/1     Running   0          6m37s
```

Access WordPress in browser:
```
http://localhost:30007
```

Both pods are healthy. The Service routes external traffic on port `30007` to the pods internally on port `80`.

---

## Task 4 — Scale the Deployment

```bash
kubectl scale deployment wordpress --replicas=4
```
```
deployment.apps/wordpress scaled
```

```bash
kubectl get pods
```
```
NAME                         READY   STATUS              RESTARTS   AGE
wordpress-848c87c44f-8mdmg   0/1     ContainerCreating   0          6s
wordpress-848c87c44f-frtlt   1/1     Running             0          6s
wordpress-848c87c44f-h5542   1/1     Running             0          6m56s
wordpress-848c87c44f-n9kps   1/1     Running             0          6m56s
```

2 new pods were created instantly. Within seconds all 4 showed `Running`.

---

## Task 5 — Self-Healing Demonstration

### Step 1 — delete one pod (simulate crash)

```bash
kubectl delete pod wordpress-848c87c44f-8mdmg
```
```
pod "wordpress-848c87c44f-8mdmg" deleted from default namespace
```

### Step 2 — check pods immediately

```bash
kubectl get pods
```
```
NAME                         READY   STATUS              RESTARTS   AGE
wordpress-848c87c44f-frtlt   1/1     Running             0          45s
wordpress-848c87c44f-h5542   1/1     Running             0          7m35s
wordpress-848c87c44f-n9kps   1/1     Running             0          7m35s
wordpress-848c87c44f-xxvhg   0/1     ContainerCreating   0          4s  ← NEW
```

### Step 3 — verify recovery

```bash
kubectl get pods
```
```
NAME                         READY   STATUS    RESTARTS   AGE
wordpress-848c87c44f-frtlt   1/1     Running   0          55s
wordpress-848c87c44f-h5542   1/1     Running   0          7m45s
wordpress-848c87c44f-n9kps   1/1     Running   0          7m45s
wordpress-848c87c44f-xxvhg   1/1     Running   0          14s  ← RECOVERED
```

**Observation:** Pod `8mdmg` was killed. Kubernetes instantly created `xxvhg` as a replacement. Total replica count stayed at 4 — no manual action required.

---

## Swarm vs Kubernetes

| Feature | Docker Swarm | Kubernetes |
|---|---|---|
| Setup | Very easy | More complex |
| Scaling | Basic | Advanced (auto-scaling) |
| Ecosystem | Small | Huge |
| Industry use | Rare | Standard |
| Self-healing | Yes (basic) | Yes (advanced) |

---

## Commands Quick Reference

```bash
kubectl apply -f <file.yaml>                    # create/update resource
kubectl get pods                                # list pods
kubectl get svc                                 # list services
kubectl get nodes                               # list cluster nodes
kubectl scale deployment <name> --replicas=N    # scale
kubectl delete pod <pod-name>                   # delete a pod
kubectl describe pod <pod-name>                 # debug details
kubectl delete -f <file.yaml>                   # remove resource
```

---

## Conclusion

This experiment demonstrated Kubernetes container orchestration using Docker Desktop's built-in cluster. A WordPress Deployment was created with 2 replicas, exposed via a NodePort Service, and scaled to 4 replicas with a single command. The self-healing capability was verified by deleting a pod — Kubernetes detected the change and automatically launched a replacement within seconds, maintaining the desired replica count of 4 at all times. Compared to Docker Swarm, Kubernetes provides a more powerful and industry-standard approach to container orchestration.

---

*Experiment 12 — Kubernetes Container Orchestration | DevOps Lab*
