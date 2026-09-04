### Namespace:

Namespace is a way to isolate workloads in a logical way. Like application wise or anyother category wise.

If everything resides in the same namepsace, it will be hard to maintain isolation and accidental deletion could happen. We can't have security or RBAC restrictions.

### POD communication from diferent namespace:

For communication from one namespace pod to another namespace pod. We need a fully qualified name.
We can use clusterIp service url.

service-name.namespace.svc.cluster - different namepsace
service-name:port or service-name.svc.cluster - same namespace
PodIP - This can be connected from one namepsace to another. But PodIps are ephermal.

Ip adress is cluster wide, but svc cluster host url is namespace specific.




