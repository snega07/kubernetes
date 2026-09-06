so slector in replicaset helps to connnect to the pod with matching labels and make sure desired number of replicas are running. Also ownerrefrence in replicaset says which deplyment owns this replicaset. 

The ReplicaSet selector identifies the Pods it should manage based on labels and ensures the desired number of replicas are running. The ReplicaSet's ownerReference identifies the Deployment that owns it.

selector:
  matchLabels:
    app: nginx

Deployment
    │
    │ ownerReference
    ▼
ReplicaSet
    │
    │ selector
    ▼
Pods

Hands On:

static and manual pod.
delete and create scheduler pod and observe the difference.
Work on labels and slector. Use command --show-labels