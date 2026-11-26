## Kubeadm do all this during kubeadm init:

![alt text](image.png)

| Communication                     | Certificate used                    |
| --------------------------------- | ----------------------------------- |
| kubectl → API server              | API server presents `apiserver.crt` |
| API server → etcd                 | `apiserver-etcd-client.crt`         |
| API server → kubelet              | `apiserver-kubelet-client.crt`      |
| API server → extension API server | `front-proxy-client.crt`            |
| etcd member → etcd member         | `peer.crt`                          |
| etcd health check → etcd          | `healthcheck-client.crt`            |

Each component has its client and server certificate and use the client trust chain CA mounted in its container to verify the client certs during connections. Flow is both MTLS and one-way TLS.


| Communication                     | TLS authentication                                                  |
| --------------------------------- | ------------------------------------------------------------------- |
| kubectl → API server              | Server-authenticated TLS; mTLS if kubectl uses a client certificate |
| API server → etcd                 | mTLS                                                                |
| API server → kubelet              | Typically mTLS                                                      |
| etcd member → etcd member         | mTLS                                                                |
| API server → extension API server | TLS, with client authentication depending on configuration          |

**kubectl authentication methods:**

| Method              | Description                                             |
| ------------------- | ------------------------------------------------------- |
| Client certificates | Authenticate using a client certificate and private key |
| Bearer tokens       | Authenticate using a token                              |
| Other mechanisms    | Other supported authentication mechanisms               |

### **Manual Kubernetes certificate management – Summary**

**1. Create your own CA**

```
ca.key  # CA private key
ca.crt  # CA public certificate
```

Keep `ca.key` securely on a separate CA machine.

**2. Generate a separate keypair for each component**

For example:

```
apiserver.key
etcd-server.key
etcd-peer.key
```

**3. Generate CSRs**

Each component generates its CSR using its own private key.

**4. Sign CSRs using your CA**

Use `ca.key` to sign each CSR and generate the corresponding certificate.

```
apiserver.crt
etcd-server.crt
etcd-peer.crt
```

**5. Distribute certificates to respective nodes**

Transfer:

- Component's private key
- Component's signed certificate
- Required trusted CA certificate(s)

**6. Configure Kubernetes components**

- Store certificates on the host.
- Mount them into the respective component containers.

- Configure certificate and CA paths through component flags.
kube-apiserver \
  --tls-cert-file=/etc/kubernetes/pki/apiserver.crt \
  --tls-private-key-file=/etc/kubernetes/pki/apiserver.key \
  --client-ca-file=/etc/kubernetes/pki/ca.crt

### **Overall workflow**

**Your CA (ca.key + ca.crt)**

**Generate component keypairs**

**Generate CSRs**

**CA signs CSRs**

**Distribute signed certificates + keys + trusted CA**

Configure Kubernetes components

**Remember:**

- Private key: Never share with the CA.
- CSR: Send to the CA for signing.
- Signed certificate: Return to the component.
- CA certificate: Distribute to components that need to verify certificates issued by that CA.
- CA private key: Never distribute to Kubernetes nodes.