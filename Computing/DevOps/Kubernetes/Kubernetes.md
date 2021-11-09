# Kubernetes
According to the [Kubernetes](https://kubernetes.io/) website,

> _"Kubernetes is an open-source system for automating deployment, scaling, and management of containerized applications."_

**Kubernetes** comes from the Greek word **κυβερνήτης**, which means _helmsman_ or _ship pilot_. With this analogy in mind, we can think of Kubernetes as the pilot on a ship of containers.

Kubernetes is highly inspired by the Google [[Borg]] system, a [[Computing/DevOps/Containers/Containers]] and workload orchestrator for its global operations for more than a decade. It is an open source project written in the Go language and licensed under the [Apache License, Version 2.0](https://www.apache.org/licenses/LICENSE-2.0).

Kubernetes was started by Google and, with its v1.0 release in July 2015, Google donated it to the [[Cloud Native Computing Foundation]] (CNCF). 

New Kubernetes versions are released in 3 months cycles. The current stable version is 1.19 (as of August 2020).


![images/flower.svg](https://d33wubrfki0l68.cloudfront.net/69e55f968a6f44613384615c6a78b881bfe28bd6/42cd3/_common-resources/images/flower.svg)

[Kubernetes](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/), also known as [[k8s]], is an open-source system for automating deployment, scaling, and management of containerized applications.

It groups containers that make up an application into logical units for easy management and discovery. Kubernetes builds upon [15 years of experience of running production workloads at Google](http://queue.acm.org/detail.cfm?id=2898444), combined with best-of-breed ideas and practices from the community.

Site: [Kubernetes](https://kubernetes.io/)
Doc: [Learn Kubernetes Basics | Kubernetes](https://kubernetes.io/docs/tutorials/kubernetes-basics/)
Blog: [Kubernetes Blog | Kubernetes](https://kubernetes.io/blog/)
GitHub: [GitHub - kubernetes/kubernetes: Production-Grade [[Computing/DevOps/Containers/Containers]] Scheduling and Management](https://github.com/kubernetes/kubernetes)

 ## Kubernetes Features I
 
 Kubernetes offers a very rich set of features for container orchestration. Some of its fully supported features are:

-   **Automatic bin packing**  
    Kubernetes automatically schedules containers based on resource needs and constraints, to maximize utilization without sacrificing availability.
-   **Self-healing**  
    Kubernetes automatically replaces and reschedules containers from failed nodes. It kills and restarts containers unresponsive to health checks, based on existing rules/policy. It also prevents traffic from being routed to unresponsive containers.
-   **Horizontal scaling**  
    With Kubernetes applications are scaled manually or automatically based on CPU or custom metrics utilization.
-   **Service discovery and Load balancing**  
    Containers receive their own IP addresses from Kubernetes, while it assigns a single Domain Name System (DNS) name to a set of containers to aid in load-balancing requests across the containers of the set.

## Kubernetes Features II

Some other fully supported Kubernetes features are:

-   **Automated rollouts and rollbacks**  
    Kubernetes seamlessly rolls out and rolls back application updates and configuration changes, constantly monitoring the application's health to prevent any downtime.
-   **Secret and configuration management**  
    Kubernetes manages sensitive data and configuration details for an application separately from the container image, in order to avoid a re-build of the respective image. Secrets consist of sensitive/confidential information passed to the application without revealing the sensitive content to the stack configuration, like on GitHub.
-   **Storage orchestration**  
    Kubernetes automatically mounts software-defined storage (SDS) solutions to containers from local storage, external cloud providers, distributed storage, or network storage systems.
-   **Batch execution**  
    Kubernetes supports batch execution, long-running jobs, and replaces failed containers.

There are many additional features currently in alpha or beta phase. They will add great value to any Kubernetes deployment once they become stable features. For example, support for role-based access control ([[RBAC]]) is stable only as of the Kubernetes 1.8 release.

## Why use Kubernetes

In addition to its fully-supported features, Kubernetes is also portable and extensible. It can be deployed in many environments such as local or remote Virtual Machines, bare metal, or in public/private/hybrid/multi-cloud setups. It supports and it is supported by many 3rd party open source tools which enhance Kubernetes' capabilities and provide a feature-rich experience to its users.

Kubernetes' architecture is modular and pluggable. Not only that it orchestrates modular, decoupled microservices type applications, but also its architecture follows decoupled microservices patterns. Kubernetes' functionality can be extended by writing custom resources, operators, custom APIs, scheduling rules or plugins.

For a successful open source project, the community is as important as having great code. Kubernetes is supported by a thriving community across the world. It has more than 2,800 contributors, who, over time, have pushed over 94,000 commits. There are meet-up groups in different cities and countries which meet regularly to discuss Kubernetes and its ecosystem. There are _Special Interest Groups_ (SIGs), which focus on special topics, such as scaling, bare metal, networking, etc. We will learn more about them in our last chapter, _Kubernetes Communities_.

## Kubernetes Users

With just a few years since its debut, many enterprises of various sizes run their workloads using Kubernetes. It is a solution for workload management in banking, education, finance and investments, gaming, information technology, media and streaming, online retail, ridesharing, telecommunications, and many other industries. There are numerous user [case studies](https://kubernetes.io/case-studies/) and success stories on the Kubernetes website:

-   [BlaBlaCar](https://kubernetes.io/case-studies/blablacar/)
-   [BlackRock](https://kubernetes.io/case-studies/blackrock/)
-   [Box](https://kubernetes.io/case-studies/box/)
-   [eBay](https://www.nextplatform.com/2015/11/12/inside-ebays-shift-to-kubernetes-and-containers-atop-openstack/)
-   [Haufe Group](https://kubernetes.io/case-studies/haufegroup/)
-   [Huawei](https://kubernetes.io/case-studies/huawei/)
-   [IBM](https://kubernetes.io/case-studies/ibm/)
-   [ING](https://kubernetes.io/case-studies/ing/)
-   [Nokia](https://kubernetes.io/case-studies/nokia/)
-   [Pearson](https://kubernetes.io/case-studies/pearson/)
-   [Wikimedia](https://kubernetes.io/case-studies/wikimedia/)
-   And many more.