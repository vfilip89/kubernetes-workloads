# **Kubernetes, Minikube, and Docker Setup Guide**

This guide provides step-by-step instructions to install and configure **Minikube, Docker, and Kubernetes (kubectl)** on a local machine for development and testing purposes.

---

## **1 Install and Configure Docker**

### **1.1 Install Docker**

Docker is required for Minikube when using the **Docker driver**.

```sh
sudo apt update
sudo apt install -y docker.io
sudo systemctl enable --now docker
```

#### **Verify Docker Installation**

```sh
docker --version
docker run hello-world
```

---

## **2 Install Minikube**

Minikube provides a lightweight Kubernetes cluster for local development.

### **2.1 Download and Install Minikube**

```sh
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

#### **Verify Minikube Installation**

```sh
minikube version
```

---

## **3 Install kubectl (Kubernetes CLI)**

`kubectl` is the command-line tool for interacting with Kubernetes clusters.

### **3.1 Install kubectl**

```sh
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install kubectl /usr/local/bin/kubectl
```

#### **Verify kubectl Installation**

```sh
kubectl version --client
```

---

## **4 Start Minikube Cluster**

Once Minikube and Docker are installed, start the Minikube cluster. Here using a laptop with 4 CPUs (2 physical cores and 2 hyperthreads) and 7.6GB RAM.

### **4.1 Start Minikube**

```sh
minikube start --cpus 2 --memory 4096 --driver=docker
```

- `--cpus 2`: Allocate 2 CPUs
- `--memory 4096`: Allocate 4GB RAM
- `--driver=docker`: Use Docker as the driver

#### **Verify Minikube Cluster**

```sh
minikube status
kubectl get nodes
```

#### **Check Running Pods (Initially Empty)**

```sh
kubectl get pods -A
```

---

## **5 Stopping and Restarting Minikube**

### **5.1 Stop Minikube**

To **pause Minikube** without deleting it:

```sh
minikube pause
```

To **stop Minikube**:

```sh
minikube stop
```

### **5.2 Restart Minikube**

```sh
minikube start
```

---

## **6 Delete Minikube Cluster (If Needed)**

To remove all Minikube configurations and reset the cluster:

```sh
minikube delete --all
```

To completely remove Minikube:

```sh
sudo rm -rf ~/.minikube
```

---

## **7 Managing Kubernetes Contexts & Handling Stuck Pods**

### **7.1 Switch Kubernetes Contexts**
If you are working with multiple Kubernetes clusters, switch between them using:

```sh
kubectl config get-contexts
kubectl config use-context minikube
```

### **7.2 Deleting Stuck Pods**
Sometimes, a pod may get stuck in a `CrashLoopBackOff` or `Terminating` state. To delete and let Kubernetes recreate it:

#### **Identify the Stuck Pod**
```sh
kubectl get pods -A
```

#### **Delete the Stuck Pod**
```sh
kubectl delete pod <pod-name> -n <namespace>
```
Replace `<pod-name>` with the actual pod name and `<namespace>` with its namespace.

#### **Verify if Kubernetes Reschedules the Pod**
```sh
kubectl get pods -n <namespace>
```

If the pod doesn’t restart properly, check logs:
```sh
kubectl logs <pod-name> -n <namespace>
```

---

## **✅ Next Steps**

Now that you have **Docker, Minikube, and Kubernetes** set up:

- ✅ Deploy a Kubernetes application.
- ✅ Configure **Storage, Networking, Ingress Controllers, and Monitoring**.
- ✅ Use **Helm** to install applications.
- ✅ Explore **persistent volumes and secrets management**.

---

🚀 **Congratulations! You now have a fully functional local Kubernetes environment!** 🚀

