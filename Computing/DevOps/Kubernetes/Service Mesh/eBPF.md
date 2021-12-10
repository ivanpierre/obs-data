# eBPF

eBPF changes this equation. It allows to dynamically extend the functionality of the Linux kernel. We have been using eBPF for Cilium to build a highly efficient network, security, and observability datapath that embeds itself directly into the Linux kernel. Applying this same concept, we can solve service mesh requirements at the kernel level as well. In fact, Cilium already implements a variety of the required concepts such as identity-based security, L3-L7 observability & authorization, encryption, and load-balancing. The missing parts are now coming to Cilium. You will find details on how to join the Cilium service mesh beta program driven by the Cilium community at the end of this blog.

![eBPF Service Mesh Architecture](/media/eBPF_Service_Mesh_Architecture.png)

Some of you may wonder why the Linux kernel community is not addressing these requirements directly. eBPF has a massive advantage. eBPF code can be inserted at runtime into an existing Linux kernel similar to a Linux kernel module, but unlike a kernel module, it can be done in a secure and portable manner. This allows for eBPF implementation to continue to evolving with the service mesh community. New kernel versions would take years to make it into the hands of users. eBPF is the critical technology that allows the Linux kernel to keep up with the rapidly evolving cloud native technology stack.

## eBPF-based L7 Tracing & Metrics without Sidecars

Let’s look at L7 tracing and metrics observability as a concrete example of how an eBPF-based service mesh has a massive impact on preserving low latency and keeping the cost of observability low. Applications teams rely on application visibility and monitoring as a fundamental requirements these, this includes capabilities such as request tracing, HTTP response rates, and service latency information. However, this observability should come at no significant cost (latency, complexity, resources, …).

![eBPF-based L7 Visibility](/media/eBPF-based_L7_Visibility.png)

In the benchmark below, we see early measurements of how implementing HTTP visibility with eBPF or a sidecar approach affects latency. The setup is running a steady 10K HTTP requests per second over a fixed number of connections between two pods running on two different nodes and measures the mean latency for the requests.

![Latency Benchmark eBPF-based vs Sidecar-based L7 visibility](/media/Latency_Benchmark_eBPF-based_vs_Sidecar-based_L7_visibility.png)

We are deliberately not mentioning the specific proxy used in these measurements because it does not matter. The results are almost identical for all proxies we have tested. To be clear, this is not about whether Envoy, Linkerd, Nginx, or another proxy is faster. There are differences in the mentioned proxies but they are insignificant in comparison to the cost of injecting the proxy in the first place. Almost none of the overhead is coming from the logic in the proxy itself. The overhead is added by injecting the proxy, redirecting network traffic to it, terminating connections, and initiating new connections.

These early measurements make it obvious that an eBPF-based in-kernel approach is extremely promising and may deliver on the desire to implement fully transparent service mesh functionality at no significant overhead.

## eBPF Accelerated Per-Node Proxy

More and more use-cases can be covered with this eBPF-only approach and thus completely void the L4 proxy. For some use cases, a proxy is still needed. For example when connections need to be spliced, when TLS termination is being performed, or for some forms of HTTP authorization.

Our eBPF service mesh efforts will continue to focus on areas where the most can be gained from a performance perspective. You may not mind terminating a connection with a proxy once as traffic flows into the cluster if you have to perform TLS termination anyway. However, you will care much more about the impact of injecting two proxies into the path of every single connection just to extract HTTP metrics and tracing data.

When a use case cannot be implemented with an eBPF-only approach, the mesh can fall back onto a per-node proxy model that directly integrates the proxy with the socket layer of the kernel.

![eBPF per-node Proxy](/media/eBPF_per-node_Proxy.png)

Instead of relying on network-level redirection, eBPF can inject the proxy directly at the socket level, keeping paths short. In the case of Cilium, Envoy is being used although from an architecture perspective, any proxy could be integrated into this model. Conceptually, this allows to extend the concept of a Linux kernel network namespace directly into the concept of an Envoy listener configuration and turn Envoy into a multi-tenant proxy.

## Sidecar vs per-Node Proxy

Even when a proxy is needed, the cost of proxy will vary depending on the architecture deployed. Let’s look at a per-node proxy model compared to a sidecar model and see how they compare.

### Proxies per Connection

The number of network connections required will vary depending on whether proxies are in the picture. The simplest scenario is the sidecar-free model which implies no changes to the number of network connections. A single connection will serve the requests and eBPF will provide service mesh capabilities such as tracing or load-balancing on the existing connection.

![eBPF-based model](/media/eBPF-based_model.png)

Providing the same functionality with a sidecar model requires injecting a proxy twice into the connection which results in three connections that need to be maintained. This results in increased overhead and multiplication of required memory for all the additional socket buffers which manifest in higher service to service latency. This is the sidecar overhead we have seen earlier in the sidecarless L7 visibility section.

![eBPF-based model](/media/eBPF-based_model-1.png)

Switching to a per-node proxy model allows us to get rid of one of the proxies because we no longer rely on running a sidecar inside of each workload. Still less ideal than no additional required connections, but better than always requiring two additional connections.

![eBPF-based model](/media/eBPF-based_model-1.png)

### Total number of proxies required

Running a sidecar in each workload can result in a large number of proxies. Even if each individual proxy instance is pretty optimized in its memory footprint, the sheer quantity of instances will result in a heavy total impact. Moreover, each proxy maintains data structures such as routing and endpoint tables which grow as the cluster grows, so the memory consumption per proxy will be higher the larger the cluster gets. Some service meshes today attempt to solve this by pushing partial routing tables to individual proxies, limiting where they can route to.

![Number of proxies](/media/Number_of_proxies.png)

Let's assume 30 pods/node in a 500 node cluster, a sidecar based architecture will require to run 15K proxies. With 70MB of memory consumed per proxy (already assuming heavily optimized routing tables), this still results in 1.5TB of memory consumed by all sidecars in the cluster. In a per-node model, with the same assumed memory footprint per proxy, the 500 proxies will consume no more than 34GB of memory.

### Multi-Tenancy

As we move from a sidecar model to a per-node model, the proxy will serve connections for multiple applications. The proxy has to become multi-tenant aware. This is exactly the same transition that has happened as we switched from using individual virtual machines to using containers instead. As we stopped using an entirely separate copy of the operating system running in each virtual machine and started sharing the operating system with multiple applications, Linux has to become multi-tenant aware. This is why namespaces and cgroups exist. Without them, a single container could consume all of the resources of a system and containers could access each other's filesystems in an uncontrolled manner.

![Envoy Namespaces](/media/Envoy_Namespaces.png)

Wouldn't it be great if this behaved exactly the same for network resources at the service mesh level? Envoy already has an initial concept of namespaces, they are called listeners. Listeners can carry individual configurations and operate independently. This will open entirely new possibilities: all of a sudden, we can easily control resource consumption and establish fair queueing rules, and distribute the available resources either equally to all applications or according to specified rules. This can and should look exactly the same as the way we define CPU and memory constraints of applications in Kubernetes today. If you want to learn more about this topic, I've spoken at EnvoyCon about this ([Envoy Namespaces - Operating an Envoy-based Service Mesh at a Fraction of the Cost, Thomas Graf, EnvoyCon 2019](https://www.youtube.com/watch?v=08opgZkdYIw)).