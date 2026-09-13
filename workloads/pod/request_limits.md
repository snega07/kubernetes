### Why Pod request and limit?

We need to specify memory and CPU limit for each container. Without appropriate resource limits, a container may consume excessive resources and cause node resource pressure, potentially affecting other workloads and leading to eviction or OOM conditions. Which restricts other pod to get into OOM killed or CPU issue throttling the running POD.

### How?

Metric server installation and its usage

We can specify resources for a container like requests memory, cpu and limits memory, cpu.

**limit:** higher bound
**request:** lower bound

Requests influence scheduling, while limits constrain runtime consumption. If a container reaches its memory limit, it may be OOMKilled; if the node as a whole runs short of memory, the node enters memory pressure and Kubernetes may evict Pods.

                    Node memory pressure
                            │
                            ↓
                  Which Pods are using
                  excessive memory?
                            │
                 ┌──────────┴──────────┐
                 ↓                     ↓
          Exceeding request?     Within request?
                 │                     │
                 ↓                     ↓
          More likely to          Less likely
            be evicted             to evict

**OOM Killed:**

When the container needs more memory then the configured limit it will be Out of memory killed.

**CPU Throttling**

When the container needs more CPU then the configured limit it will thottled. Takes lots of time to process the request and respond. Application latency will be increased.

**ADD ons:**

To know the current CPU and memory utilization of pods and nodes. We can use top command. This needs metric server installatin which give real time data at the moment.

                  Kubernetes
                      │
        ┌─────────────┴─────────────┐
        │                           │
   Requests/Limits             Metrics Server
        │                           │
        │                           │
   "How much can/should        "How much is it
    this Pod consume?"          actually using?"
        │                           │
        ↓                           ↓
   Scheduler + kubelet         kubectl top
   enforcement                 HPA metrics

**NOTE:**

During Pod scheduling, Kubernetes uses the resource requests to determine whether the Pod can fit on a node. The limits are not used for scheduling in the normal case; they are used to enforce the maximum resource consumption of the container after it starts.

If the application temporarily needs more CPU than its request, it can use additional CPU up to its limit (if available). If it exceeds its CPU limit, it will be throttled.

If the application exceeds its memory limit, the container can be OOMKilled.

