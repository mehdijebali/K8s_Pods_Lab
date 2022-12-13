# Create & Manage pods in K8s
In this tutorial, we will work with pod K8s Object through different set of Labs.
## Container Resources
#### Resource Request
We define a resource request that allows user to speciy an expected resource (RAM, CPU) that can be consumed by a container/pod. In fact, Kube Scheduler will manage resource request and avoid scheduling on node, which don’t have enough resources. Note that containers are allowed to use more or less than the requested resource because Resource Request is used to manage scheduling only.
```
apiVersion: v1
kind: Pod
metadata:
  name: frontend
spec:
  containers:
  - name: app
    image: alpine
    command: ["sleep", "3600"]
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
```
#### Resource limit
Resource Limits used to Limit the container’s resource consumption at RunTime (RAM, CPU). If a pod exceeds the resource limits, K8s will throttle the pod and terminates it.
```
apiVersion: v1
kind: Pod
metadata:
  name: frontend-limit
spec:
  containers:
  - name: app
    image: alpine
    command: ["sleep", "3600"]
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
```
## Health Check 
K8s provides number of features to monitor containers. Which helps to identify the container state & auto restart in case of failure.
#### Liveness Probe
Liveness probe aims to determine the Container State, it improves container monitoring mechanism. We can execute Two types of Liveness probes:
- Run Command in Container
```
spec:
  containers:
    - name: liveness
      .
      .
      .
      livenessProbe:
        exec:
          command:
            - cat
            - /tmp/healthcheck
        initialDelaySeconds: 5
        periodSeconds: 5
```
- Periodic HTTP Health Check
```
spec:
  containers:
    - name: liveness
      .
      .
      .
      livenessProbe:
        httpGet:
          path: /
          port: 80
        initialDelaySeconds: 3
        periodSeconds: 3
```
**initialDelaySeconds** stands for how long to wait before sending a probe after a container starts. **periodSeconds** refers to how often a probe will be sent.
#### StartUp Probe
StartUp probe is similar to liveness probe but it is used for application with long startup time. StartUp probe runs at container StartUp and stop running once container success. Once the startup probe has succeeded once, the liveness probe takes over to provide a fast response to container deadlocks.
```
spec:
  containers:
    - name: startup
      .
      .
      .
      startupProbe:
        httpGet:
          path: /
          port: 80
        failureThreshold: 30
        periodSeconds: 10
```
**failureThreshold** stands for number of times K8s will try before giving up. In this case, Application will have a maximum of 5 minutes (30*10 seconds) to ﬁnish its startup.
#### Readiness Probe
Readiness is used to detect if a container is ready to accept trafﬁc because sometimes application might need to load large data or conﬁguration ﬁles during startup, or depend on external services after startup. As a result, the pod will not receive any traffic until the container passes the readiness probe. 
```
spec:
  containers:
    - name: readiness
    .
    .
    .
      readinessProbe:
        httpGet:
          path: /
          port: 80
        initialDelaySeconds: 3
        periodSeconds: 3
```
## Node Affinity
## Restart Policies
## Scheduling
## Pod Lifecycle
## Multi-Containers