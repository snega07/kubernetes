taint toleration

effect: noschedule, noexecute, noexecute

kubectl taint node node_name gpu=true:NoSchedule(effect)

kubectl run redis --image=redis --dry-run=client -o yaml > redis.yaml

toleration
key value effect

Equlas:
Used when you want the key and value to match exactly.

Exists:
Used when you only care that the taint key exists. The value doesn't matter.

Hands on:
taint the node
schedule pod with tolreation
no toleration
remove taint from node

Taints & Toleration gurantees: That what pod can get schedule on that node. But the tolerated pods can have the possiblity to get scheduled on other nodes. This keeps the node clean but pod may get scheudle somewhere.

Selector:

Gives pod option to say it must get scheudled on particular node using the label.

nodeSelector -> in pod.yaml
lable -> in node
pod/node affinity selector

limitaion on nodeselector : we can't use AND OR operator

Reuqest flow from browser to aplication

so on this concet there is



taint toleration effect

node selector and label

pod affinity anti affinity - works with node label

node affinity anti affintiy -works with node label

| Operator         | Meaning                                              |
| ---------------- | ---------------------------------------------------- |
| **In**           | Label value must be one of the specified values      |
| **NotIn**        | Label value must NOT be one of the specified values  |
| **Exists**       | Label key must exist; value doesn't matter           |
| **DoesNotExist** | Label key must not exist                             |
| **Gt**           | Label value must be greater than the specified value |
| **Lt**           | Label value must be less than the specified value    |

 

| Concept               | Works with  | Purpose                                                                                                           |
| --------------------- | ----------- | ----------------------------------------------------------------------------------------------------------------- |
| **Node Label**        | Node        | Adds information to a node                                                                                        |
| **Node Selector**     | Node labels | Simple way to select nodes                                                                                        |
| **Node Affinity**     | Node labels | Advanced node selection **or avoidance** using operators like `In`, `NotIn`, `Exists`, `DoesNotExist`, `Gt`, `Lt` |
| **Pod Affinity**      | Pod labels  | Place a Pod near other matching Pods                                                                              |
| **Pod Anti-Affinity** | Pod labels  | Keep a Pod away from other matching Pods                                                                          |
| **Taint**             | Node        | Repels Pods from a node                                                                                           |
| **Toleration**        | Pod         | Allows a Pod to tolerate a node's taint                                                                           |
| **Taint Effect**      | Taint       | Determines how the taint affects new/existing Pods                                                                |


required/preferd during scheduling ignore during execution

| Concept             | Meaning                                    |
| ------------------- | ------------------------------------------ |
| `nodeSelectorTerms` | **OR** between alternative node conditions |
| `matchExpressions`  | Conditions within a term                   |
| `required`          | **Must** satisfy                           |
| `preferred`         | **Prefer** if possible                     |
| `weight`            | How strongly to prefer it, **1–100**       |

