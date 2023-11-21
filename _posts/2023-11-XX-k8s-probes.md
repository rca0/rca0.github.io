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

When you need yours systems to detect a faulty condition in your applications and remediate the situation immediately and automatically. Health Checks could be a great fit to know if an instance of your application is working or not working within distributed systems.

Working with containers in a distributed systems could be hard to managed, by default, kuberentes starts to send traffic to a Pod when all the containers inside the Pod start, and restarts containers when they crash.

Kuberentes Probes are used to detect:
* Containers that haven't started yet and can't serve traffic.
* Containers that are overwhelmed and can't serve additional traffic.
* Containers that are completely dead and not serving any traffic.

Kubernetes offer 3 types of health checkers.

> StartupProbe
> LivenessProbe
> ReadinessProbe

Before we delve into Probes, it's importante to know more about how's pod health checks works. PODs are the smallest deployable units in the Kubernetes World, and they wrap containers and containers host applications. The PodSpec define containers and you can have multi-containers within Pods.

## POD Health Checks

|              | **Liveness**          | **Readiness**               |
| ===          | ===                   | ===                         |
| On Failues   | Kill Container        | Stop sending traffic to Pod |
| Check types  | http, exec, tcpSocket | http, exec, tcpSocket       |

The probe will perform a diagnostic on the container's health using the following handlers:

* **ExecAction**: executes a command inside the container.
* **TCPSocketAction**: performs a TCP Check against the container's IP address on a specified port.
* **HTTPGetAction**: performs an HTTP GET request on the container's IP address.

The probe can returns; 

* **Success**: the diagnostic passed on the container.
* **Fail**: the container failed the diagnostic and will restart according to its restart policy.
* **Unknown**: the diagnostic failed and no action will be taken.

## LivenessProbe

The `LivenessProbe` is used by `Kubelet` to knows when to restart a container. It indicates whether the container is running inside the Pod. E.g. Liveness probes could catch a deadlock, when application is running, but unable to make progress. Restarting a container in such a state can help to make the application more available despite bugs. When your app is dead, Kuberentes removes the Pod and starts a new one to replace it. If a container does not provide a Liveness Probe the default state is success.

In a simple words, if the health check application is not working, By using a Liveness Probe, Kuberentes detects that the application is no longer serving requests and restarts the Pod.

![Liveness Probe](https://raw.githubusercontent.com/rca0/rca0.github.io/master/_posts/assets/livenessprobe.gif)



## ReadinessProbe

The Readiness Probe method evaluate and check the dependencies of your application before your app is ready, imagine if you want to check if your database is running and receiving traffic, or you want to send request in an external API dependency and verify if the communication is ok, figure out this issues before the application is running, it would be great, isn't?

Therefore, Readiness comes to solve those problems, in a simple words, you could check if your dependencies is ready, follow some examples;

> Database requests
> Authorization requests
> Service for telemetry requests

Other good example when use Readiness Probe, if you have an application might need to load large data or configuration files during startup, or depende external services after startup, and you want to not kill your application, the pod with readinessProbe configuration will report that they are not ready does not receive traffic throught kubernetes services.

Unlike a Liveness Probe, a Readiness Probe doesn't kill the container. If the Readiness Probe fails, Kubernetes simply hides the container's Pod from corresponding Services, so that no traffic is redirected to it.

In other words, when the ReadinessProbe is ready, the Kubernetes Service allow to send traffic to the Pod. If ReadinessProbe starts to fail, Kubernetes stops sending traffic to the Pod until it passes.

A side-effect of using Readiness Probe is that they can increase the time it takes to update deployments.

![Readiness Probe](https://raw.githubusercontent.com/rca0/rca0.github.io/master/_posts/assets/readinessprobe.gif)


# When NOT use Liveness and Readiness



## Examples


## Declaration e.g (pod.yaml)

* LivenessProbe

```yaml
...
livenessProbe: 
    failureThreshold: 3     # how many failures to accept before failing
    timeoutSeconds: 1       # how long to wait for a response
    initialDelaySeconds: 3  # how long to wait before checking
    periodSeconds: 3        # how long to wait between checks
    successThreshold: 1     # how many successes to hit before accepting
    httpGet:                # make an HTTP request
        path: /_healthz     # endpoint to hit
        port: 8080          # port to use
        scheme: HTTP        # or HTTPS
...
```

* ReadinessProbe

```yaml
...
ReadinessProbe:
    initialDelaySeconds: 3  # how long to wait before checking
    periodSeconds: 3        # how long to wait between checks
    successThreshold: 1     # how many successes to hit before accepting
    failureThreshold: 1     # how many failures to accept before failing
    timeoutSeconds: 1       # how long to wait for a response
    httpGet:                # make an HTTP request
        path: /_healthz     # endpoint to hit
        port: 8080          # port to use
        scheme: HTTP        # or HTTPS
...
```

# Takeaways

better reliability 
higher uptime

