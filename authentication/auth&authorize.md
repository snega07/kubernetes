**Authentication**

Authentication is the process of verifying the identity of a user or system. It ensures that the entity requesting access is who they claim to be. Common methods of authentication include:

- Password-based authentication: Users provide a username and password to verify their identity.
- Token-based authentication: Users are issued a token that they must present to access resources.
- Certificate-based authentication: Users present a digital certificate to prove their identity.
- Asymmertic Encryption, symmertric encrytion

**Authentication to a kubernetes cluster is done using kubeconfig** file which holds the cluster details and the API server endpoint. And the user context to authenticate to the cluster including their client certificates.

**Authorization**

Authorization is the ability or access to say what action this user can perform in the system.

- ABAC - Attribute based authentication(associate set of permission and add this policies to the api server config) We need to restart API server everytime.
- RBAC - Create role and role binding to assign to the user 
- NODE - NODE to interact with each using node based authorization.
- Webhook - OPA -> to validate the incoming request most on resource creation to maintain the cluster policy.

**Static pod yaml of apiserver:**

In the apiserver pod yaml. in the container spec commands we have the authorization mode mentioned. It will be applied in the same order.

For API server to communicate with other component it holds the certificates client, server and CA certs in the command section of pod. We can see the actual certificate under /etc/kubernetes/pki

- authorization-mode: NODE, RBAC

options:

AlwaysAllow
AlwaysDeny

- authorization-policy-file






