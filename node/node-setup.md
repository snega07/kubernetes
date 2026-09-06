### Cluster Node Setup:

Usually during our Kubernetes cluster setup: We first install the kubelet, containerd, kubectl, kubeadm

kubectl and kubeadm are different — they are CLI tools, not continuously running services.

kubeadm → used to bootstrap/initialize the cluster
kubectl → used to interact with the Kubernetes API
kubelet → continuously runs on every node
containerd → container runtime running on every node

**Kubelet, containerd** -> run as a system process. **systemctl status containerd(or)kubelet** gives the status of the process. 
**ps -aux | grep kubelet**
**ps -aux | grep containerd**

**Control plane component:**
API server, etcd, scheduler, controller

All these are created as static pod by placing the manifest in directory monitored by kubelet **/etc/kuberntes/manifests**

**Worker-Node**

install kubelet, containerd, kube-proxy/CNI/application pods

The cluster has a CNI plugin such as Cilium, Calico, Flannel, etc., whose components provide Pod networking.

**Watch and query**

All the communication between these component happens through API-server. API-server act as central hub all these component watches API server for changes. Components commonly watch/query the API server for the state they care about. Here watching mean APi-server notify the specifc component whenever any changes occur respective to thier work.

The component sends a watch request to the API server:

Scheduler
   │
   │  WATCH Pods
   ▼
API Server

The API server keeps that watch connection open. When a relevant object changes, it sends an event back:

Pod created
    ↓
API Server
    ↓
Watch event
    ↓
Scheduler

