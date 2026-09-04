**init containers:**

- Init containers are used to configure any pre-requisites before main container starts.
- Main container starts after successfull initialization of all init container. Else main container will be in waiting state.
- We can have more than one init container.
- Init containers execute sequentially in the order they are defined.

**sidecar/helper containers**

- This keeps on running with main container.
- Act as a helper for main container.
- Example: service mesh envoy proxy, Log collector, Configuration agent, Monitoring agent

**main container**

- Contains the actual application workload.
- Normally runs continuously for a Deployment-managed application.
- Starts only after all regular init containers have completed successfully.


Containers in the same Pod share the Pod's network namespace and volumes, while each container has its own filesystem/process namespace. CPU and memory are allocated to individual containers based on their resource requests/limits, although they are competing for resources on the same node.

**Commands:**

To get logs of specific container

kubectl logs my-pod -c <container-name>
kubectl exec -it my-pod -c <container-name>

Note:

Since deployment wants pod/container to be running all the time.
Incase if we use busybox. The pod will keep on restarting as the container restart policy is always. when the contaner exit it will restart it. Which makes pod to get into crashloop.