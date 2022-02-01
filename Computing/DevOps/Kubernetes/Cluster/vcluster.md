# What are Virtual Kubernetes Clusters?

Virtual clusters are fully working Kubernetes clusters that run on top of other Kubernetes clusters. Compared to fully separate "real" clusters, virtual clusters do not have their own node pools. Instead, they are scheduling workloads inside the underlying cluster while having their own separate control plane.

![vcluster Architecture](https://www.vcluster.com/docs/media/diagrams/vcluster-architecture.svg)

vcluster - Architecture

The virtual cluster itself only consists of the core Kubernetes components: API server, controller manager and storage backend (such as etcd, sqlite, mysql etc.). To reduce virtual cluster overhead, vcluster builds by default on [k3s](https://k3s.io/), which is a fully working, certified, lightweight Kubernetes distribution that compiles the Kubernetes components into a single binary and disables all not needed Kubernetes features, such as the pod scheduler or certain controllers.

Besides k3s, other Kubernetes distributions such as [k0s and vanilla k8s are supported](https://www.vcluster.com/docs/operator/other-distributions). In addition to the control plane, there is also a Kubernetes hypervisor that replaces the Kubernetes scheduler and emulates a fully working Kubernetes setup in the virtual cluster. This component syncs a handful of core resources that are essential for cluster functionality between the virtual and host cluster:

-   **Pods**: All pods that are started in the virtual cluster are rewritten and then started in the namespace of the virtual cluster in the host cluster. Service account tokens, environment variables, DNS and other configurations are exchanged to point to the virtual cluster instead of the host cluster. Within the pod, it so seems that the pod is started within the virtual cluster instead of the host cluster.
-   **Services**: All services and endpoints are rewritten and created in the namespace of the virtual cluster in the host cluster. The virtual and host cluster share the same service cluster IPs. This also means that a service in the host cluster can be reached from within the virtual cluster without any performance penalties.
-   **PersistentVolumeClaims**: If persistent volume claims are created in the virtual cluster, they will be mutated and created in the namespace of the virtual cluster in the host cluster. If they are bound in the host cluster, the corresponding persistent volume information will be synced back to the virtual cluster.
-   **Configmaps & Secrets**: ConfigMaps or secrets in the virtual cluster that are mounted to pods will be synced to the host cluster, all other configmaps or secrets will purely stay in the virtual cluster.
-   **Other Resources**: Deployments, statefulsets, CRDs, service accounts etc. are **NOT** synced to the host cluster and purely exist in the virtual cluster.

In addition to the synchronization of virtual and host cluster resources, the hypervisor proxies certain Kubernetes API requests to the host cluster, such as pod port forwarding or container command execution. It essentially acts as a reverse proxy for the virtual cluster.

## Why use Virtual Kubernetes Clusters?[#](https://www.vcluster.com/docs/what-are-virtual-clusters#why-use-virtual-kubernetes-clusters "Direct link to heading")

Virtual clusters solve many of the problems that namespaces present, such as:

-   **Cluster-Scoped Resources**: Certain resources live globally in the cluster, and you can’t isolate them using namespaces. For example, installing istio or any other operator in different versions is not possible within a single cluster.
    
-   **Shared Kubernetes control plane**: the API server, etcd, scheduler, and controller-manager are shared in a single Kubernetes cluster. Request or storage rate-limiting based on a namespace is very hard and faulty configuration might bring down the whole cluster.
    

Virtual clusters also provide more stability than namespaces in many situations. The virtual cluster creates its own Kubernetes resource objects, which are stored in its own data store. The host cluster has no knowledge of these resources.

Isolation like this is excellent for resiliency. Engineers who use namespace-based isolation often still need access to cluster-scoped resources like cluster roles, shared CRDs or persistent volumes. If an engineer breaks something in one of these shared resources, it will likely fail for all the teams that rely on it.

Because you can have many virtual clusters within a single cluster, they are much cheaper than the traditional Kubernetes clusters, and they require lower management and maintenance efforts. This makes them ideal for running experiments, continuous integration, and setting up sandbox environments.

Finally, virtual clusters can be configured independently of the physical cluster. This is great for multi-tenancy, like giving your customers the ability to spin up a new environment or quickly setting up demo applications for your sales team.

![vcluster Comparison](https://www.vcluster.com/docs/media/vcluster-comparison.png)

- [vcluster Home](https://www.vcluster.com/)
- [GitHub - loft-sh/vcluster: vcluster - Create fully functional virtual Kubernetes clusters - Each vcluster runs inside a namespace of the underlying k8s cluster. It's cheaper than creating separate full-blown clusters and it offers better multi-tenancy and isolation than regular namespaces.](https://github.com/loft-sh/vcluster)
- [vcluster docs | Virtual Clusters for Kubernetes](https://www.vcluster.com/docs/what-are-virtual-clusters)

