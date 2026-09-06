Pod is the smallest deployable unit in Kubernetes. Pod is highly immutable. Most of the spec of pod can't be uodated once created. Allowed to modify image version alone.

A Pod can contain one or more containers. Containers run inside the Pod, and the Pod provides a shared execution environment including:

Shared network namespace/IP
Shared volumes
Container configuration such as environment variables
Labels and annotations are attached to the Pod
Resource requests/limits are defined for containers

⚠️ Better wording than "Pod wraps the container":
A Pod is a wrapper/abstraction around one or more containers.

Imperative - Tells k8s what action to perform.Mostly for Quick testing, debugging,One-time operations and during development.
Declarative - Tells k8s whats the final state we want. More suitable for repo in git, cicd..

| Command                      | Resource doesn't exist             | Resource already exists |
| ---------------------------- | ----------------------             |----------------------- |
| `kubectl create -f pod.yaml` | ✅ Creates, imperative             | ❌ Error                 |
| `kubectl apply -f pod.yaml`  | ✅ Creates, declarative            | ✅ Updates               |

When created using apply, future update is alowed. As first creation using apply maintain last configured annotaion that stores and compare previous state of the resource. If created using create command this annotation is omitted. So we can't use apply to uodate further.

kubectl describe pod -> shows pod creation events, node details, image, env, labels and other details about pod
kubectl get pod - o wide -> shows pod in the namespace with IP details
kubectl get pod -o yaml -> give pod definition in yaml format
kubectl log pod-name -> shows pod container logs
kubectl exec -it pod-name -- sh -> opens interactive terminal
kubectl exec -it <pod-name> -c <container-name> -- sh
kubectl run nginx --image=nginx --dry-run=client -> will just dry run won't vcreate the pod
kubectl edit pod-name -> helps to edit live object
kubectl scale --replicas=10 replicaset-name
output to file: This inclused status filed which we can remove. This is added for all the running pod to show the pod status

kubectl run nginx --image=nginx:latest --dry-run=client -o yaml > pod.yaml


