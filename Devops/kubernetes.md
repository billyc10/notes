# Kubernetes

# Architecture
![architecture](image.png)

- The control plane is the _brains_ of the operation.
- Control plane nodes are usually odd numbered (3 in our case), to prevent split-brain situations (where nodes lose contact with each other and write different things)
- The API server is the entrypoint to the server
    - Exposes a RESTful API to post yaml configuration files (mainfests) to
- The manifest describes desired state of a K8s application
    - Container image
    - Exposed ports
    - How many pod replicas
- The scheduler assigns pods to worker nodes (on the bottom right). Checks if node has compatibility and capacity
- etcd holds the state of the system
- The controller manager keeps track of what's going on in the cluster: observed state = desired state?
    - The controllers are constantly polling the cluster to observe

Root Nodes:
- `kubelet` application that runs on each node in the cluster. Ensures app is running in pods, and sends current state back to control plane
- `kube-proxy` maintains network rules
    - IP address, ingress, egress
- Container runtime. K8s just orchestrates containers, but the running of the containers is offloaded

## Containers
![k8s containers](image-1.png)
K8s talks to a Container Runtime Interface, which then talks to the container runtime. However, K8s doesn't need Docker's full suite of functionality (e.g. it already has its own networking functionality), so Docker has released its own container runtime called Container D, which K8s uses. Use of Docker via docker shim is no longer supported.

## Pods
Smallest unit in K8s
Typically one container per pod, with own IP address.
- _Can_ run 2 containers per pod, but usually as sidecars, doing monitoring etc. Containers within a pod can talk to each other via `localhost`
- One container could also be a _service mesh_, taking care of non business-logic things.

You would typically not deploy just a single pod by itself. You wouldn't be making use of K8's orchestration features, and if the pod dies then that's it.

## Deployments
Deployments provide features such as:
- self healing
- scaling
- zero-downtime rollouts
    - Rollouts create new pods, and delete old pods only once the new pod is active
- versioned rollbacks
    - By default last 10 replica sets are kept around
to a pod.

![deployment](image-2.png)

### Manifests
Everything in K8s is described as manifest files (which are posted to the control plane API)


# Troubleshooting
```shell
kubectl describe replicaset <replicaset>
```
