Control Plane Node

API server -> any request first reach API server. API server interacts with other component. Main entry point.

ETCD -> API server stores the definition in etcd. Key value data store. It stores the cluster state, resources. Any changes, update or resource created will be updated to ETCD. Only API server could interact with ETCD and any info required about the resources like replicas, pods, deployment will be retrieved from ETCD.

Scheduler -> Scheduler receives the request from API server. Checkes the nodes for resources availablity and schedule the pod.
Controller Manager -> Helps to maintain the desired number of replica. Namepsace controler, node, deployment, replication controller. Make sure that all the controllers are running fine and monitor the resource and helps with autoscaling decision.

Worker Node

ContainerD
Kubelet -> Helps to restart, signals containerd to pull image and start a container. Receives request from API server to do chnages in resources.
Kube-proxy -> Enables networking within the node. Helps pod to communicate with each and and configure IPtable rules. Pod to Pod networking.

User -> kubectl cli -> interacts with API server -> authenticate, validate and check RBAC -> Sends to etcd and store the entry then signals back to API server -> Then scheduler keeps on monitoring the control plane so it find that it needs to schedule a pod it go on and helps to schedule the pod in worker node signal to API server-> API sevrer asks Kubelet to create the pod sends response to API server-> API server adds entry to etcd pod created and user get the response back to user. 

