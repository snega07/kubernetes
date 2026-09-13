### ConfigMap

When we want to store values that are not sensitive and will be consumed by our container. We can store them as a configMap. Key value pair or yaml file can be loaded into the container from configMap when the pod starts.

It is stored as plain text in the etcd not encryted, So it is more suitable for non-sensitive data.

Consume:

We can mount it as volume, refer envFrom configMap directly or load the data one by one using the keyName.

### Secrets

When we want to access sensitive data inside the container. We should go with secrets. They are encrytpted at rest in the ETCD database.

Consume:

We can mount it as volume, refer envFrom secrets directly or load the data one by one using the keyName.