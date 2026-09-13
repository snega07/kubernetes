### Auto Scaling:

When the request per seconds or load to the application increases. It is our responsibiliy to scale up the service either vertical or horizontally based on the nature of the application.

Both k8s workload(POD) as well as node(infra) can be implemented to auto-scale.

**Note:**

- If you want to scale based on other metrics like kafka message depth, or other events. You to go for KEDA.
- Cron scheduled based autoscaling.

**HPA(Horizontal Pod Autoscaler):**

HPA is used to scale the pods horizontally based on the CPU utilization or other selected metrics. It automatically adjusts the number of pods in a replication controller, deployment, or replica set based on observed CPU utilization.

- HPA does not require an add-on, but CPU/memory metrics normally require Metrics Server. For custom/external metrics, you need an appropriate metrics adapter.
- HPA can scale based on CPU, memory, custom, or external metrics—not just CPU/memory.
- Used when the application is more critical and it requires 0 downtime.
- Comes with K8s default workloads. No add on installation required.
- Scales based on cpu and memory.
- By default, HPA supports resource metrics such as CPU and memory, but Kubernetes needs a metrics provider such as Metrics Server to expose those metrics through the Metrics API. HPA itself doesn't collect the metrics.

             ┌──────────────────┐
             │ Observe CPU usage │
             └────────┬─────────┘
                      ↓
             Calculate utilization
                      ↓
             Compare with target
                      ↓
        ┌─────────────┴─────────────┐
        ↓                           ↓
   Above target                Below target
        ↓                           ↓
   Scale up                    Scale down
        └─────────────┬─────────────┘
                      ↓
                 Check again

Uitilization percentage is calculated for CPU request.

Suppose:

CPU request per pod = 500m
HPA target          = 50%
Current replicas    = 2

Now both pods are using:

Pod 1 = 700m
Pod 2 = 600m

Their individual utilization is:

Pod 1 = 700 / 500 × 100 = 140%
Pod 2 = 600 / 500 × 100 = 120%

Average:

(140% + 120%) / 2 = 130%

Now HPA calculates approximately:

Desired replicas
= 2 × (130 / 50)
= 5.2

HPA rounds this according to its scaling algorithm, so it would target roughly 6 replicas (subject to its other constraints and stabilization behavior).

**VPA(Vertical Pod Autoscaler):**

VPA is used to scale the pods vertically based on the CPU and memory utilization. It automatically adjusts the resource requests and limits of the pods based on observed CPU and memory usage.

- When application can have some down time. As changing the resource require recreation of pod.
- Requires add on installtion. As k8s doesn't have native resource definition or controller.
- Scales based on resource utilization.

**Serverless**

Serverless is a way to scale down application to 0. This helps use the available resource effectively. If service doesn't require to be running always and need to be started based on on-demand request then we can go with serverless.

- It primarily uses request concurrency and/or request rate depending on configuration/autoscaling settings.
- Need to install knative serving.(Openshift native operator)
- Supported for application which starts quickly and doesn't require to be running always.
- Scaling metrics is based on cuncurrent request per seconds.
- Supports traffic splitting to different revision of deployment workloads.
