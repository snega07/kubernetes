Pod is the smallest deployable unit in Kubernetes.

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

kubectl describe pod -> shows pod creation events, node details, image, env, labels and other details about pod
kubectl get pod - o wide -> shows pod in the namespace with IP details
kubectl get pod -o yaml -> give pod definition in yaml format
kubectl log pod-name -> shows pod container logs
kubectl exec -it pod-name -- sh -> opens interactive terminal
kubectl exec -it <pod-name> -c <container-name> -- sh
kubectl run nginx --image=nginx --dry-run=client -> will just dry run won't vcreate the pod
kubectl get pods nginx-pod --show-labels -> shows particular pod object
kubectl edit pod-name -> helps to edit live object
kubectl scale --replicas=10 replicaset-name
output to file: This inclused status filed which we can remove. This is added for all the running pod to show the pod status

kubectl run nginx --image=nginx:latest --dry-run=client -o yaml > pod.yaml
