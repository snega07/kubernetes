Service is a way to provide POD a static way to access. Since POD Ips are ephermal. Default service type is clusterIP. Below are the type of service available.

Service holds the endpoints which has the Ips that are available for the underlying POD that matches the label. Also this endpoints will be updated whenever the pod get recreated. 

Nodeport
ClusterIp
Loadbalancer
External



### nodeport:
- expose the service and it is accesible from a specific port of a node and forward the traffic to target port of the pod.
- acessible only when the node and its network is rechable for the client. Stays within the node network.
- 30000 to 32767
- can be accessed using nodeIP:port-number. Also each service needs to have unique port.

- We can have more than one set of port of a services. Port filed is a list.

name       → identifier for the port
port       → Service port
targetPort → Pod/container destination port
nodePort   → Node's exposed port

nodePort: The port opened externally on all your cluster nodes. By default, Kubernetes forces this to be within the 30000–32767 range. If left blank, the cluster assigns a random one.

port: The internal port used by other pods inside the cluster to access this service.

targetPort: The actual port your application container is listening on.


### ClusterIP
- default service type is clusterIp
- Pod IPs are ephermal.
- To make one pod communicate with another we need a static IP. We need to have service of type clusterIp. 
- This service helps with communication using this service internal domain and url.

format of url:

http://my-service:80
http://my-service.myapp.svc.cluster.local:80

my-service          → Service name
myapp               → Namespace
svc                 → Service
cluster.local       → Cluster DNS domain

Pod in namespace: frontend

        │
        │ http://backend
        │       ❌ if backend is in another namespace backend
        │
        ▼

http://backend.backend.svc.cluster.local or http://backend.backend:8080
        │
        ▼
Backend Service

### Loadbalancer

- Nodeport mode service are accessible only if node network is accesible. Also each application must be exposed in unique port. It is hard to share the NodeIP of each node where the ppd runs to the end user, which is not a good practice.
- Create external loadbalancer and refer this loadbalancer from k8s service. The application can be accessed from outside world.
- Loadbalancer use clusterIp internally. Also it creates a node port mapping for this service.
- We need a cloud provider, which provides loadbalancer. Then will get an external IP.

### External

ExternalName Service is used to provide a Kubernetes DNS name/alias for an external service. It allows applications running inside the cluster to access an external service using the Kubernetes Service name instead of directly using the external DNS name

- If the external DNS changes, we can simply update the DNS in k8s service of type external.
- External is to make application running in a pod to access outer application that is exposed using a DNS name.
-  Like if our application use a database endpoint with externalName: my.database.example.com. We can create a external service and make our application access it using this external service http://my-database or my-database.namespace.svc.cluster.local.
- This is only to make our pod access external service.


| Type             | ClusterIP | NodePort  | External LB | Selects Pods |
| ---------------- | --------- | --------- | ----------- | ------------ |
| **ClusterIP**    | ✅         | ❌         | ❌           | ✅            |
| **NodePort**     | ✅         | ✅         | ❌           | ✅            |
| **LoadBalancer** | Usually ✅ | Usually ✅ | ✅           | ✅            |
| **ExternalName** | ❌         | ❌         | ❌           | ❌            |
