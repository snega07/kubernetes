### ConfigMap

When we want to store values that are not sensitive and will be consumed by our container. We can store them as a configMap. Key value pair or configuration file can be loaded into the container from configMap when the pod starts.

It is stored as plain text in the etcd not encryted/encoded by default, So it is more suitable for non-sensitive data.

**Examples:**

- Application configuration
- Environment-specific values
- URLs
- Feature flags
- Configuration files

A ConfigMap can be consumed by a Pod in several ways:

**Consuming ConfigMap**

**As environment variables**
envFrom:
  - configMapRef:
      name: app-config
This loads all keys from the ConfigMap as environment variables.

**Individual key as an environment variable**

env:
  - name: APP_MODE
    valueFrom:
      configMapKeyRef:
        name: app-config
        key: APP_MODE

**As a volume**

The ConfigMap can be mounted as a volume inside the container.

Each key can appear as a file containing its corresponding value.

### Secrets

When we need to store sensitive data such as passwords, tokens, API keys, or certificates, Kubernetes Secrets can be used.

By default, Secret data is base64-encoded, not encrypted. For stronger security, Kubernetes can be configured with encryption at rest, so Secret data is encrypted when stored in etcd.

**Consuming Secrets**

Secrets can be consumed in the same general ways as ConfigMaps:

**All keys as environment variables**

envFrom:
  - secretRef:
      name: app-secret

**Individual key as an environment variable**

env:
  - name: DB_PASSWORD
    valueFrom:
      secretKeyRef:
        name: app-secret
        key: DB_PASSWORD

**As a volume**

The Secret can be mounted as a volume, where each key is exposed as a file inside the container.


