### Static and Manual Scheduling

Whenever we apply a yaml file to create a pod. Scheduler decides which node this pod needs to be scheduled. 
Some cases we need static pod. Like if u take control plane component(apiserver, etcd, scheduler, controller manager) they themselves are pod. How these pods are scheduled and running. Thats where Static pod comes into picture.

Kubelet checks for a directory to check pod manifest. Whenver it find pod defintion yaml in that directory, it will run that pod. The special this is this pod.yaml will have the node name where it needs to be scheduled.

The concept is simple:
1. **Static Pod* Kubelet checks a directory(/etc/kubernetes/manifest) to check pod manifest in the node. Whenever it finds pod definition yaml in that directory, it will run that pod in the same node.
3. *If **node name** is mentioned pod will scheduled by **kubelet** itself. If **no node name** is mentioned, scheduler will takes care of picking the node and running the pod.