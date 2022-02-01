# Container orchestrators

## Distributions

With enterprises containerizing their applications and moving them to the [[Cloud]], there is a growing demand for [[Container Orchestration]] solutions. While there are many solutions available, some are mere re-distributions of well-established [[Container Orchestration]] tools, enriched with features and, sometimes, with certain limitations in flexibility.

Although not exhaustive, the list below provides a few different [[Container Orchestration]] tools and services available today:

-   **Amazon Elastic [[Computing/DevOps/Containers/Containers]] Service**  
    [Amazon Elastic Container Service](https://aws.amazon.com/ecs/) (ECS) is a hosted service provided by [Amazon Web Services](https://aws.amazon.com/) (AWS) to run [[Docker]] containers at scale on its infrastructure.
-   **Azure [[Computing/DevOps/Containers/Containers]] Instances**  
    [Azure Container Instance](https://azure.microsoft.com/en-us/services/container-instances/) (ACI) is a basic [[Container Orchestration]] service provided by [Microsoft Azure](https://azure.microsoft.com/en-us/).
-   **Azure Service Fabric**  
    [Azure Service Fabric](https://azure.microsoft.com/en-us/services/service-fabric/) is an open source [[Container orchestrators]] provided by [Microsoft Azure](https://azure.microsoft.com/en-us/).
-   **Kubernetes**  
    [Kubernetes](https://kubernetes.io/) is an open source orchestration tool, originally started by Google, today part of the [Cloud Native Computing Foundation](https://www.cncf.io/) (CNCF) project.
-   **Marathon**  
    [Marathon](https://mesosphere.github.io/marathon/) is a framework to run containers at scale on [Apache Mesos](https://mesos.apache.org/).
-   **Nomad**  
    [Nomad](https://www.nomadproject.io/) is the [[Computing/DevOps/Containers/Containers]] and workload orchestrator provided by [HashiCorp](https://www.hashicorp.com/).
-   **[[Docker]] Swarm**  
    [Docker Swarm](https://docs.docker.com/engine/swarm/) is a [[Computing/DevOps/Containers/Containers]] orchestrator provided by [Docker, Inc](https://www.docker.com/). It is part of [Docker Engine](https://docs.docker.com/engine/).

## Why Use Container Orchestrators?

Although we can manually maintain a couple of containers or write scripts to manage the lifecycle of dozens of containers, orchestrators make things much easier for operators especially when it comes to managing hundreds and thousands of containers running on a global infrastructure.

Most [[Computing/DevOps/Containers/Containers]] orchestrators can:

-   Group hosts together while creating a [[Computing/DevOps/Kubernetes/Cluster/Cluster]]
-   Schedule containers to run on hosts in the [[Computing/DevOps/Kubernetes/Cluster/Cluster]] based on resources availability
-   Enable containers in a [[Computing/DevOps/Kubernetes/Cluster/Cluster]] to communicate with each other regardless of the host they are deployed to in the [[Computing/DevOps/Kubernetes/Cluster/Cluster]]
-   Bind containers and storage resources
-   Group sets of similar containers and bind them to load-balancing constructs to simplify access to containerized applications by creating a level of abstraction between the containers and the user
-   Manage and optimize resource usage
-   Allow for implementation of policies to secure access to applications running inside containers.

With all these configurable yet flexible features, container orchestrators are an obvious choice when it comes to managing containerized applications at scale. In this course, we will explore **[[Computing/DevOps/Kubernetes/Kubernetes]]**, one of the most in-demand [[Container Orchestration]] tools available today.

## Where to Deploy Container Orchestrators?
Most container orchestrators can be deployed on the infrastructure of our choice; on bare metal, Virtual Machines, on-premises, on public and hybrid [[Cloud]]. [[Computing/DevOps/Kubernetes/Kubernetes]], for example, can be deployed on a workstation, with or without a local hypervisor such as Oracle VirtualBox, inside a company's data center, in the [[Cloud]] on AWS Elastic Compute Cloud (EC2) instances, Google Compute Engine (GCE) VMs, DigitalOcean Droplets, OpenStack, etc.

There are turnkey solutions which allow [[Computing/DevOps/Kubernetes/Kubernetes]] clusters to be installed, with only a few commands, on top of [[Cloud]] Infrastructures-as-a-Service, such as GCE, AWS EC2, [[Docker]] Enterprise, IBM Cloud, Rancher, VMware Tanzu, and multi-cloud solutions through IBM Cloud Private or StackPointCloud.

Last but not least, there is the managed [[Container Orchestration]] as-a-Service, more specifically the managed [[Computing/DevOps/Kubernetes/Kubernetes]] as-a-Service solution, offered and hosted by the major [[Cloud]] providers, such as [Amazon Elastic Kubernetes Service](https://aws.amazon.com/eks/) (Amazon EKS), [Azure Kubernetes Service](https://azure.microsoft.com/en-us/services/kubernetes-service/) (AKS), [DigitalOcean Kubernetes](https://www.digitalocean.com/products/kubernetes/), [Google Kubernetes Engine](https://cloud.google.com/kubernetes-engine/) (GKE), [IBM Cloud Kubernetes Service](https://www.ibm.com/cloud/container-service), [Oracle Container Engine for Kubernetes](https://cloud.oracle.com/containers/kubernetes-engine), or [VMware Tanzu Kubernetes Grid](https://tanzu.vmware.com/kubernetes-grid).