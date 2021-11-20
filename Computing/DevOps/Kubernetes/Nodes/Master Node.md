# # Master Node

The **master node** provides a running environment for the **control plane** responsible for managing the state of a [[Computing/DevOps/Kubernetes/Kubernetes]] [[Cluster]], and it is the brain behind all operations inside the [[Cluster]]. The control plane components are agents with very distinct roles in the [[Cluster]]'s management. In order to communicate with the [[Computing/DevOps/Kubernetes/Kubernetes]] [[Cluster]], users send requests to the control plane via a Command Line Interface (CLI) tool, a Web User-Interface (Web UI) Dashboard, or Application Programming Interface (API).

It is important to keep the control plane running at all costs. Losing the control plane may introduce downtime, causing service disruption to clients, with possible loss of business. To ensure the control plane's fault tolerance, master node replicas can be added to the [[Cluster]], configured in High-Availability (HA) mode. While only one of the master nodes is dedicated to actively manage the [[Cluster]], the control plane components stay in sync across the master node replicas. This type of configuration adds resiliency to the [[Cluster]]'s control plane, should the active master node fail.

To persist the [[Computing/DevOps/Kubernetes/Kubernetes]] [[Cluster]]'s state, all [[Cluster]] configuration data is saved to [etcd](https://etcd.io/). **[[etcd]]** is a distributed key-value store which only holds [[Cluster]] state related data, no client workload data. [[etcd]] may be configured on the master node ([stacked](https://kubernetes.io/docs/setup/independent/ha-topology/#stacked-etcd-topology) topology), or on its dedicated host ([external](https://kubernetes.io/docs/setup/independent/ha-topology/#external-etcd-topology) topology) to help reduce the chances of data store loss by decoupling it from the other control plane agents.

With [[Stacked etcd topology]], HA master node replicas ensure the [[etcd]] data store's resiliency as well. However, that is not the case with [[External etcd topology]], where the [[etcd]] hosts have to be separately replicated for HA, a configuration that introduces the need for additional hardware.

## Master Node Components

A master node runs the following control plane components:

-   [[API Server]]
-   [[Scheduler]]
-   Controller Managers
-   Data Store.

In addition, the master node runs:

-   Container Runtime
-   Node Agent
-   Proxy.

