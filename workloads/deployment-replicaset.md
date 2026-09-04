Whenever you make a change to the Pod template in a Deployment, Kubernetes creates a new ReplicaSet or replication controller.

Deployment = Kubernetes → ReplicaSet → Pods
DeploymentConfig = OpenShift → ReplicationController → Pods

Older Kubernetes
ReplicationController
       ↓
     Pods


Modern Kubernetes
Deployment
    ↓
ReplicaSet
    ↓
   Pods

Replication controller - replication/self-healing/scaling
replicaset - improved replication/self-healing/scaling
deployment - Replicaset + rollout/update/rollback management


### Replication controller:(legacy)
Helps to maintain a desired number of replicas(count of pod)
Has loadbalancing capacity
Pods can span to multiple nodes based on the resource availability check by scheduler.

### Replicaset: (newer)

Helps to maintain existing pod as well, using selector match label.
API version is different

All resource in k8s has below structure:

apiVersion:
kind:
metadata:
    name:
    labels:
spec:
  template:# metadata and spec of the pod
  metadata:
    name: pod-name
  spec:
    container definiton
  replicas: 1

* To see the API version of k8s resource.

kubectl explain rc, pod, deployment.
kubectl explain is used to learn the fields and structure of Kubernetes API resources directly from Kubernetes.

kubectl explain pod ->It gives you information about the Pod resource and its fields.

### Deployment

Deployment -> Replicaset -> PODs

If we don't have deployment. Any update or changes to pod container spec will lead to whole deleteion of pod and user will face downtime.

But with deployment maintaining replicaset. We can do rolling update or rollback easier. So user won't face downtime.

kubectl rollout history deploy/deployment-name
kubectl rollout undo deployment-name


| Feature              | ReplicationController          | ReplicaSet                     |
| -------------------- | ------------------------------ | ------------------------------ |
| Purpose              | Maintains desired Pod replicas | Maintains desired Pod replicas |
| API                  | `ReplicationController`        | `ReplicaSet`                   |
| Selector             | **Equality-based** only        | **Set-based + equality-based** |
| Example selector     | `app=nginx`                    | `app in (nginx, apache)`       |
| Used with Deployment | ❌ No                           | ✅ Yes                          |
| Modern Kubernetes    | Legacy                         | Recommended                    |
| Replacement          | —                              | Replaced RC                    |


