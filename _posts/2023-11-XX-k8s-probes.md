---
layout: post
title: Kubernetes Probes
short-description: xxx...
date: 2023-11-XX
---

# Kubernetes Probes

In this post I will cover the Kubernetes Probe.

If your first time here, and yet don't know what it Kubernetes, i will cover a little bit about it. Kubernetes it is a robust and extensive OpenSource platform for managing containerized workloads and services. Kuberenetes provides you with a framework to run distributed systems resiliently. It takes care of scaling and failover for your application, provides deployments patterns and more.

Kubernetes provides you:

* Service Discovery and Load Balancing
* Storage Orchestration
* Automated Rollout and Rollbacks
* Automatic Bin Packing
* Self-Healing
* Secret and Configuration Management
* Batch Execution
* Horizontal Scaling
* IPv4/IPv6 dual-stack
* Designed for Extensibility

Kuberentes is very complex with a lot of componentes, i really love the way that k8s was designed, and i hope to write a lot more about this. 

## Let's Get Started

Kubernetes offer 3 types of health checkers.

> StartupProbe
> LivenessProbe
> ReadinessProbe

## POD Health Checks

PODs are the smallest deployable units in the Kuberentes world.
PODs wrap containers and containers host applications.
Within PodSpec we can define containers 
Multi-container Pods.

|                            | **Liveness**          | **Readiness**               |
| ===                        | ===                   | ===                         |
| On Failues                 | Kill Container        | Stop sending traffic to Pod |
| Check types                | http, exec, tcpSocket | http, exec, tcpSocket       |

The probe will perform a diagnostic on the container's health using the following handlers:

* ExecAction: executes a command inside the container.
* TCPSocketAction: performs a TCP Check against the container's IP address on a specified port.
* HTTPGetAction: performs an HTTP GET request on the container's IP address.

The probe can returns; 

* Success: the diagnostic passed on the container.
* Fail: the container failed the diagnostic and will restart according to its restart policy.
* Unknown: the diagnostic failed and no action will be taken.

## LivenessProbe

## ReadinessProbe

When your systems has a set of shared dependencis, you can use the ReadinessProbe method to evalute and check the dependencias of your application.
e.g.

> Database requests
> Authorization requests
> Service for telemetry requests


# When NOT use Liveness and Readiness



## Examples


## Declaration e.g (pod.yaml)

* LivenessProbe

```yaml
...
livenessProbe: 
    failureThreshold: 3
    httpGet:
        path: /_healthz
        port: 8080
...
```

* ReadinessProbe

```yaml
...
ReadinessProbe: 
    httpGet:
        path: /_healthz
        port: 8080
...
```
