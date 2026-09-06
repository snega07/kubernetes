### Taint and Toleration:

- Taint is used add a specific mark to a node and says only the pod which tolerates this taint must be scheduled here. We add a key value pair value with effect as taint to a node. 

- A taint repels Pods; a toleration allows a Pod to tolerate the taint; but a toleration alone does not determine where the Pod will be scheduled.

``` yaml
Key=value:effect
taint:  kubectl taint node kind-dev-worker2 gpu.enabled=true:NoExecute
untaint : kubectl taint node kind-dev-worker2 gpu.enabled=true:NoExecute-
```
**allowed effects:**
- NoSchedule = existing pod will keep on running. New Pod must tolreate the taint
- NoExecute = Taint affects both new and old pods that doesn't tolerate. More restrictive.
- PreferNoSchedule = is a soft avoidance, not strictly "accept only when no other nodes are available." The scheduler tries to avoid the tainted node but can still schedule there.

**Tolerations:**

We must add tolerations in the pod definition. Pods without a matching toleration are prevented/restricted from running there. A toleration only gives permission; it does not force placement.

**Operator:** 

- Equal : value must match the key and value of the node taint.
- Exists: Expect the tint key must present in the node. Does not care about the value.

**tolerationseconds:** say how long it could tolerate the taint. Only used with NoExecute.

Default tolerations are commonly added to Pods for the not-ready and unreachable NoExecute taints. To make sure when the worker node is unreachable or not ready this will keep the pod tolerated for 5mins(300s). Post that it will be evicted and based on the controller it will get scheduled in healthy node.

Worker node will get attached with this taint automatically when they are unreachable/not-ready.

``` bash
node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
```

``` yaml
  tolerations:
  - key: "gpu.enabled"
    operator: "Equals"
    value: "true"
    effect: "NoExecute"
    tolerationSeconds: 120
```

### Label & Selector:

Toleration alone does not gurantee that always pod will get scheduled in the tainted node. 

To direct a Pod to a particular type of node, we can:

- Add a label to the Node.
- Use nodeSelector in the Pod specification.

``` bash
add label: kubectl label node kind-dev-worker cpu=high
remove label: kubectl label node kind-dev-worker cpu=high-
kubectl get nodes --show-labels
```
If only one node has this label, the Pod will be scheduled on that node (assuming it satisfies all other scheduling requirements).

If multiple nodes have the label, the Pod can be scheduled on any suitable matching node.

It only supports a simple key value pair. No advanced filtering options and operators are supported. Thats where affinity comes into picture.

``` yaml
  nodeSelector:
    env: dev
```

### Affinity

Since node selector does not support advanced operators. Node affinity/anti-affinity will provide a way to make use of logical selection of nodes as per our requirement.

| Operator         | Meaning                                              |
| ---------------- | ---------------------------------------------------- |
| **In**           | Label value must be one of the specified values      |
| **NotIn**        | Label value must NOT be one of the specified values  |
| **Exists**       | Label key must exist; value doesn't matter           |
| **DoesNotExist** | Label key must not exist                             |
| **Gt**           | Label value must be greater than the specified value |
| **Lt**           | Label value must be less than the specified value    |

- Label the node
- add affinity to pod and configure selector terms with match expression using the logical operator.
- anti-affinity can be achieved using NotIn and DoesNotExist

**requiredDuringSchedulingIgnoredDuringExecution:** Stricly require to satisfy the affinity selector term.
**preferedDuringSchedulingIgnoredDuringExecution:** less Strict require to satisfy the affinity selector term. Pod will get schedlued in nodes that does not satisfy the affinity condition.
**IgnoredDuringExecution:** label of the node changes post pod scheduling. It wont affect the running pod. 

required   → MUST satisfy → no weight
preferred  → TRY to satisfy → weight (1–100)

```yaml
 affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: disktype
            operator: NotIn
            values:
            - ssd 
            - hhd
          - key: cpu
            operator: DoesNotExist
        - matchExpressions:
          - key: region
            operator: In
            values:
            - us-west-1

```
**Prefred**

``` yaml
affinity:
  nodeAffinity:
    preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 100
        preference:
          matchExpressions:
            - key: disktype
              operator: In
              values:
                - ssd

      - weight: 50
        preference:
          matchExpressions:
            - key: environment
              operator: In
              values:
                - prod
```
You are saying:

"I prefer an SSD node very strongly (100), and I prefer a PROD node less strongly (50)."

How scheduler thinks

Suppose:

| Node     | SSD preference | PROD preference |   Total |
| -------- | -------------: | --------------: | ------: |
| worker-1 |            100 |               0 | **100** |
| worker-2 |              0 |              50 |  **50** |
| worker-3 |            100 |              50 | **150** |

The scheduler gives higher score to nodes satisfying preferred rules.

So it would prefer worker-3 because it satisfies both preferences.

**CheatSheet:**

values inside In       → OR
values inside NotIn    → "none of these values"
expressions in 1 term  → AND
separate terms         → OR

| Concept             | Meaning                                    |
| ------------------- | ------------------------------------------ |
| `nodeSelectorTerms` | **OR** between alternative node conditions |
| `matchExpressions`  | Conditions within a term                   |
| `required`          | **Must** satisfy                           |
| `preferred`         | **Prefer** if possible                     |
| `weight`            | How strongly to prefer it, **1–100**       |

| Concept               | Works with  | Purpose                                                                                     |
| --------------------- | ----------- | ------------------------------------------------------------------------------------------- |
| **Node Label**        | Node        | Adds identifying information/attributes to a node                                           |
| **Node Selector**     | Node labels | Simple node selection using **key-value equality**                                          |
| **Node Affinity**     | Node labels | Advanced node selection/avoidance using `In`, `NotIn`, `Exists`, `DoesNotExist`, `Gt`, `Lt` |
| **Pod Affinity**      | Pod labels  | Schedule a Pod **near** other matching Pods                                                 |
| **Pod Anti-Affinity** | Pod labels  | Schedule a Pod **away from** other matching Pods                                            |
| **Taint**             | Node        | Repels/restricts Pods from a node                                                           |
| **Toleration**        | Pod         | Allows a Pod to **tolerate** a matching node taint                                          |
| **Taint Effect**      | Taint       | Defines what happens to Pods when they encounter the taint                                  |

### Pod Affinity ⭐

"Schedule my Pod near Pods having a particular label."

### Pod Anti-Affinity ⭐

"Schedule my Pod away from Pods having a particular label."

For example, you might have:

Node 1
 ├── frontend-1
 └── frontend-2

Node 2
 └── backend-1