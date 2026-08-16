Why k8s?

Docker containers does not handle below:

- Service discovery and load balancing
- High availability
- Autoscaling
- Auto healing
- Networking

When a container goes down. A person has to login to the VM or server where this container is running and debug the issue and scale the container. This is hard if we have multiple container running for a micro service application.

- No rolling update. Leads to downtime
- No autohealing, K8s kubelet helps to retsart the pod container if it is idle. Docker provied basic restart always.
- Service discover. ClusterIp service of k8s helps service to service communication with internal DNS name. Docker support bridge network and http://containe-name:port format.
- Better networking.
- No Autoscaling. With k8s we can implement Horizontal and vertical autoscaling. Which scale based on different metrics.
- Expose service to public using Ingress. With container it is hard to do this.
- Kube-proxy do loadbalancn=ing to the available number of pods. No external load balancer required.