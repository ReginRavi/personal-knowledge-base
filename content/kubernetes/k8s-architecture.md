---
title: "Kubernetes Architecture Explained"
date: 2026-02-07T12:00:00+05:30
tags: ["kubernetes", "architecture", "control-plane"]
draft: false
---

![The Control Plane Node and Worker Node](../Images/The%20Control%20Plane%20Node%20and%20Worker%20Node.png)

## The Control Plane Node: The Brain 🧠

This is where the decision-making happens. Every Kubernetes cluster has one or more control plane nodes that oversee everything in the cluster.

Here's how it all fits together:

### API Server (api):

Think of it as the front desk of Kubernetes. Every kubectl command or internal component interaction goes through the API server. It validates your requests and routes them to the right place.

### Controller Manager (c-m):

The automation genius. If your app's desired state (like 3 replicas) doesn’t match reality, the controller manager steps in to create, delete, or update resources.

### Scheduler (sched):

New pod? Cool. The scheduler finds the best worker node for it, considering factors like resources, affinity, and taints. It's all about optimal placement.

### etcd:

The brain's memory. This is a highly consistent key-value store that keeps track of everything in the cluster. If etcd is down, Kubernetes forgets the cluster's state.

### kubelet on Control Plane:

Just like on worker nodes, the kubelet on the control plane ensures containers running here are healthy and up-to-date.



## Worker Node: The Muscles 💪

While the control plane is busy planning and deciding, the worker nodes do the actual work.

Here's what happens under the hood:

kubelet:

The node's manager. It takes orders from the API server

## Control Plane Components

Kubernetes control plane consists of several key components that manage the cluster state:

### API Server
The API server is the central management entity that receives all REST requests for modifications to pods, services, and other Kubernetes objects.

### etcd
A consistent and highly-available key-value store used as Kubernetes' backing store for all cluster data.

### Scheduler
Watches for newly created Pods with no assigned node and selects a node for them to run on.

### Controller Manager
Runs controller processes that regulate the state of the cluster.

## Worker Nodes

Worker nodes run the containerized applications:

- **kubelet**: Agent that ensures containers are running in a Pod
- **kube-proxy**: Network proxy maintaining network rules
- **Container Runtime**: Software responsible for running containers (e.g., containerd, CRI-O)

## Example Pod Definition

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
```

This architecture enables Kubernetes to orchestrate containerized applications at scale.

## ✅ Quick Answers Corner

Q: What does this command return?
kubectl get pods -l env=prod -n staging
A: Lists all Pods in the staging namespace that are labeled env=prod.
Bonus Challenge (Think Like a Pro)
 
Q: What does this command do?
kubectl get pods -l 'tier in (frontend,backend)' -n dev 
--field-selector=status.phase=Running
A: Lists all running Pods in the dev namespace with the label tier = frontend or tier = backend.
So far, you’ve launched Pods and exposed them with Services. But here’s the real-world problem… 
What happens if a Pod crashes?
What if you want 5 copies of the same app running?
What if you need to update your app without downtime?
That’s where Deployments and ReplicaSets come in.

![Deployments and ReplicaSets](../Images/Deployments%20and%20Replicasets.png)

What’s a ReplicaSet?

A ReplicaSet ensures that the correct number of identical Pods is always running.
If a Pod crashes → a new one is created
If you scale to more replicas → more Pods are created automatically
Example:
```yaml
replicas: 3
selector:
  matchLabels:
    app: web
```

This tells Kubernetes to keep 3 Pods running that match the label app=web.
 
📌 You rarely create ReplicaSets directly — you use a Deployment to do it.
So What’s a Deployment?

A Deployment is a higher-level object that manages ReplicaSets for you.
It can:
* Create and maintain ReplicaSets
* Handle rolling updates
* Perform version rollbacks
* Define how your app should run (via the Pod Template)

📌 Think of a Deployment as your app’s “control system.”

Simple Deployment Example

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
        - name: nginx
          image: nginx:1.21
          ports:
            - containerPort: 80
```
This will:

Create a ReplicaSet with 3 Pods
Run NGINX version 1.21
Ensure all Pods are labeled app=web
What’s a Pod Template?

Inside the Deployment YAML, the template: section is your Pod Template.
It’s the blueprint used to create all future Pods in that Deployment.
If a Pod is restarted or recreated, it’s rebuilt based on this template.

## What Happens When You Update the Image?

Let’s say you update the image from nginx:1.21 to nginx:1.22.
You run:
* kubectl apply -f web-deployment.yaml
* Kubernetes will:
* Launch new Pods with the new image
* Wait for them to be healthy
* Delete the old Pods
* Complete the update — with zero downtime

## Want to Roll Back?
 
You can easily undo any update with:
kubectl rollout undo deployment web-deployment

📌 Kubernetes tracks deployment history, so you can revert anytime.

## Useful Commands to Try

* kubectl get deployments
* kubectl describe deployment web-deployment
* kubectl scale deployment web-deployment --replicas=5
* kubectl rollout status deployment web-deployment
* kubectl rollout undo deployment web-deployment
## Quick Summary

Concept	What It Does	Why It Matters
ReplicaSet	Ensures # of Pods is always running	Auto-healing, consistent scaling
Deployment	Manages ReplicaSets + updates/rollbacks	Safer changes, simplified control
Pod Template	Blueprint for new Pods	Used when recreating/replacing

## Quick Review
* What keeps Pods running? → ReplicaSet
* What helps update your app version? → Deployment
* Where do recreated Pods come from? → Pod Template