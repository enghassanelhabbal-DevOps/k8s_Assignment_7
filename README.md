# 🚀 Kubernetes Scheduling & Workloads Lab

This project demonstrates core Kubernetes scheduling techniques and workload types through hands-on tasks using Minikube.

It covers:

* Pod scheduling strategies
* Node selection & affinity
* Taints & tolerations
* QoS classes
* DaemonSets
* Static Pods

---

## 📌 Prerequisites

* Kubernetes (Minikube recommended)
* kubectl installed
* Basic understanding of Pods & YAML

---

## 🧩 Task Overview

### 🟢 Task 1: Pod Scheduling using `nodeName`

* Hard pins a Pod to a specific node
* Bypasses the scheduler

```yaml
spec:
  nodeName: minikube
```

📌 Key Insight:
Forces the Pod onto a specific node (not recommended in production)

---

### 🔵 Task 2: `nodeSelector` (Label-Based Scheduling)

Assign label to node:

```bash
kubectl label node minikube env=lab
```

Pod uses:

```yaml
nodeSelector:
  env: lab
```

📌 Behavior:

* If label exists → Pod runs
* If not → Pod stays Pending

---

### 🟣 Task 3: Taints & Tolerations

Apply taint:

```bash
kubectl taint nodes minikube test=true:NoSchedule
```

Pod without toleration:
→ Pending

Pod with toleration:

```yaml
tolerations:
  - key: "test"
    operator: "Equal"
    value: "true"
    effect: "NoSchedule"
```

→ Running

NoExecute effect:

* Evicts running Pods without toleration

📌 Key Insight:
Taints repel Pods, tolerations allow them

---

### 🟡 Task 4: Node Affinity

Required (Hard Rule):

```yaml
requiredDuringSchedulingIgnoredDuringExecution
```

Preferred (Soft Rule):

```yaml
preferredDuringSchedulingIgnoredDuringExecution
```

📌 Behavior:

* Required → must match
* Preferred → best effort

---

### 🟠 Task 5: QoS Classes

| Class      | Condition            |
| ---------- | -------------------- |
| Guaranteed | requests = limits    |
| Burstable  | requests < limits    |
| BestEffort | no resources defined |

📌 Eviction Priority:

1. BestEffort
2. Burstable
3. Guaranteed

---

### 🔴 Task 6: DaemonSet

* Ensures one Pod runs on each node

```yaml
kind: DaemonSet
```

📌 Use Cases:

* Monitoring agents
* Logging
* Security tools

---

### ⚫ Task 7: Static Pods

* Managed directly by kubelet
* Stored in:

```
/etc/kubernetes/manifests/
```

📌 Behavior:

* Automatically recreated if deleted
* Must remove manifest file to delete permanently

---

## 🔍 Verification Commands

```bash
kubectl get pods -o wide
kubectl describe pod <pod-name>
kubectl get nodes --show-labels
kubectl get ds
```

---

## 📦 Deliverables

Proof file generated using:

```bash
echo "--- Tasks Proof ---" > k8s_proof.txt
kubectl get pods -o wide >> k8s_proof.txt
```

---

## 🎯 Key Takeaways

* Kubernetes scheduling can be controlled at multiple levels
* Labels & affinity provide flexibility
* Taints enforce node protection
* QoS affects pod eviction priority
* DaemonSets are essential for node-level services
* Static Pods are managed outside the control plane

---

## 💼 Author

**Abdelrahman El Ashry**
Aspiring DevOps Engineer focused on building reliable and production-ready systems.
