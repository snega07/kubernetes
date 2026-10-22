**Kubeconfig**

When we execute a command in kubectl. How does our cluster API server authenticates the user who is interacting. This is done using the kubeconfig file.

The kubeconfig file is a configuration file that tells kubectl which clusters it can connect to, how to authenticate to them, and which cluster and user combination to use by default.

By default it is placed in the $HOME/.kube/config

Commands:

kubectl config view
kubectl config current-context
kubectl config get-contexts
kubectl config use-context cluster-name
kubectl config delete-context cluster-name

**The 4 important fields in kubeconfig**

**clusters**	Defines cluster connection details, such as the API server endpoint and CA certificate.
**users**	Defines authentication credentials or mechanisms, such as client certificates or AWS IAM token generation.
**contexts**	Connects a cluster with a user and optionally a namespace.
**current-context**	Specifies which context kubectl uses by default.