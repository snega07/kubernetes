Imperative - Tells k8s what action to perform.
Declarative - Tells k8s whats the final state we want.

| Command                      | Resource doesn't exist             | Resource already exists |
| ---------------------------- | ----------------------             |----------------------- |
| `kubectl create -f pod.yaml` | ✅ Creates, imperative             | ❌ Error                 |
| `kubectl apply -f pod.yaml`  | ✅ Creates, declarative            | ✅ Updates               |

kubectl describe pod -> shows pod creation events, node details, image, env, labels and other details about pod
kubectl get pod - o wide -> shows pod in the namespace with IP details
kubectl get pod -o yaml -> give pod definition in yaml format
kubectl log pod-name -> shows pod container logs
kubectl exec -it pod-name -- sh -> opens interactive terminal
kubectl run nginx --image=nginx --dry-run=client -> will just dry run won't vcreate the pod
kubectl get pods nginx-pod --show-labels -> shows particular pod object

output to file: This inclused status filed which we can remove. This is added for all the running pod to show the pod status

kubectl run nginx --image=nginx:latest --dry-run=client -o yaml > pod.yaml
