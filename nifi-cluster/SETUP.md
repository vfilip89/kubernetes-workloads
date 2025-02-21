# **NiFi Cluster Setup on Kubernetes**

This document provides a detailed step-by-step guide to setting up an **Apache NiFi Cluster** on Kubernetes using Minikube, Helm, and NiFiKop (NiFi Operator).

---

## **1️⃣ Prerequisites**

Before proceeding, ensure you have the following installed and configured:

- **Minikube** (Kubernetes local cluster)
- **kubectl** (Kubernetes CLI)
- **Helm** (Package manager for Kubernetes)
- **Docker** (Container runtime)
- **NiFiKop** (NiFi Operator for Kubernetes)

Verify installations:

```sh
kubectl version --client
helm version
docker --version
minikube status
```

---

## **2️⃣ Start Minikube & Setup Kubernetes**

Ensure Minikube is running before deploying NiFi.

```sh
minikube start --cpus 2 --memory 4096 --driver=docker
```

Verify the Kubernetes cluster:

```sh
kubectl get nodes
```

---

## **3️⃣ Install Cert-Manager (For SSL & Certificates)**

Cert-Manager is required for managing TLS certificates in NiFi.

### **3.1 Add Jetstack Helm Repository**

```sh
helm repo add jetstack https://charts.jetstack.io
helm repo update
```

### **3.2 Install Cert-Manager**

```sh
helm install cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.7.2
```

Verify the installation:

```sh
kubectl get pods -n cert-manager
```

---

## **4️⃣ Install NiFi Operator (NiFiKop)**

NiFiKop manages Apache NiFi clusters on Kubernetes.

### **4.1 Add NiFiKop Helm Repository**

```sh
helm repo add konpyutaika https://konpyutaika.github.io/helm-charts/
helm repo update
```

### **4.2 Install NiFi Operator**

```sh
helm install nifikop konpyutaika/nifikop \
  --namespace nifi \
  --create-namespace \
  --version 0.13.0
```

Verify the installation:

```sh
kubectl get pods -n nifi
```

---

## **5️⃣ Deploy NiFi Cluster**

### **5.1 Apply NiFi Cluster Configuration**

Create a `nifi-cluster.yaml` configuration and apply it:

```sh
kubectl apply -f kubernetes-workloads/nifi-cluster/manifests/nifi-cluster.yaml
```

Verify the NiFi cluster status:

```sh
kubectl get pods -n nifi
```

---

## **6️⃣ Access NiFi UI**

Once the NiFi cluster is running, you can access the UI:

```sh
minikube service nifi-service -n nifi
```

This will open the NiFi web interface in your default browser.

---

## **7️⃣ Manage NiFi Cluster**

### **7.1 Scaling the NiFi Cluster**

To scale the NiFi cluster up or down:

```sh
kubectl scale deployment nifi --replicas=3 -n nifi
```

### **7.2 Restarting NiFi Pods**

If any NiFi pod is stuck:

```sh
kubectl delete pod -n nifi --all
```

Kubernetes will recreate the pods automatically.

---

## **8️⃣ Uninstall & Cleanup**

To delete the NiFi cluster:

```sh
kubectl delete -f kubernetes-workloads/nifi-cluster/manifests/nifi-cluster.yaml
```

To uninstall NiFiKop:

```sh
helm uninstall nifikop -n nifi
kubectl delete namespace nifi
```

To uninstall Cert-Manager:

```sh
helm uninstall cert-manager -n cert-manager
kubectl delete namespace cert-manager
```

To stop Minikube:

```sh
minikube stop
```

To delete Minikube completely:

```sh
minikube delete --all
```

---

