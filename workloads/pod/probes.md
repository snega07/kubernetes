### Probes

Health probes is a way to check whether our application is healthy and do necessary actions to get the application to health.

- StartupProbe - Used mainly for slow-starting applications. Kubernetes waits for the startup probe to succeed before it starts running the liveness and readiness probes. If the startup probe keeps failing beyond its configured failure threshold, the container is restarted.

- ReadinessProbe - Checks whether the application is ready to receive traffic. If the readiness probe fails, Kubernetes removes the Pod from the Service's ready endpoints, so traffic is not sent to that Pod. The Pod is not restarted. Once the probe succeeds again, the Pod can receive traffic.

- LivenessProbe -  Checks whether the application is still functioning/alive. If the liveness probe fails repeatedly according to its configuration, Kubernetes restarts the container. It is useful for detecting applications that are stuck, deadlocked, or otherwise unhealthy and cannot recover on their own.

### Supported Checks

exec command: execute a command inside the container and non-zero exit code will fail the probe.
Http: we will be configuring a health check path with port.
TCP: checks whether the port is open inside the container.

### Why StartupProbe?

Initial delay seconds only provide a fixed time also doesn't check whether application has started completely and directly execute the readiness and liveness probe. But with startup probe we can mentione periodSeconds, successThreshold and failure threshold. Since application startup time may vary based on the resources it need to connect before startup. StartupProbe provides the flexibility.




| Probe         | Main question                             | Failure action                                 |
| ------------- | ----------------------------------------- | ---------------------------------------------- |
| **Startup**   | "Has the application started?"            | Restart container if startup continually fails |
| **Readiness** | "Can I send traffic to it?"               | Remove from Service endpoints                  |
| **Liveness**  | "Is the application still healthy/alive?" | Restart container                              |

| Field                 |        Default | Meaning                                                              |
| --------------------- | -------------: | -------------------------------------------------------------------- |
| `initialDelaySeconds` |  **0 seconds** | Wait this many seconds after container starts before the first probe |
| `periodSeconds`       | **10 seconds** | How often the probe runs                                             |
| `timeoutSeconds`      |   **1 second** | How long each probe is allowed to respond.                           |
|                       |                | Post initial delay seconds                                           |
| `failureThreshold`    |          **3** | Consecutive failures before the probe is considered failed           |
| `successThreshold`    |          **1** | Consecutive successes required to consider the probe successful      |

**Supported Probe Mechanisms**

1. **Exec Probe**

Executes a command inside the container.

Exit code 0 → Probe succeeds
Non-zero exit code → Probe fails

Example:

exec:
  command:
    - cat
    - /tmp/healthy

2. **HTTP Probe**

Kubernetes sends an HTTP request to the specified path and port of the Pod.

Example:

httpGet:
  path: /health
  port: 8080

Kubernetes considers HTTP status codes 200–399 successful.

For example:

GET http://<pod-ip>:8080/health
              ↓
        200 OK → Success
        500     → Failure
        Timeout → Failure

3. TCP Probe

Kubernetes attempts to establish a TCP connection to the specified port.

tcpSocket:
  port: 8080

If the TCP connection can be established → Success.

If the connection cannot be established → Failure.