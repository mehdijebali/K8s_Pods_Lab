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
## Node Affinity
## Restart Policies
## Scheduling
## Pod Lifecycle
## Multi-Containers