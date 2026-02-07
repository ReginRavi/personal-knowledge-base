---
title: "Kubernetes Architecture Explained"
date: 2026-02-07T12:00:00+05:30
tags: ["kubernetes", "architecture", "control-plane"]
draft: false
---

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
