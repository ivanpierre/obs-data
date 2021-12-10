# Sidecar Model

Today, service meshes are commonly implemented using an architecture called the sidecar model. This architecture encapsulates the code implementing the functionality described above into a layer 4 proxy, and then relies on traffic from and to services to be redirected into this so-called sidecar proxy. It is called a sidecar because there is a proxy attached to each application, much like a sidecar attaches to a motorbike.

![Sidecar-based service mesh model](/media/Sidecar-based_service_mesh_model.png)

The advantage of this architecture is that services are no longer required to implement service mesh functionality themselves. This is beneficial if many services are deployed written in different languages or if you are running 3rd-party applications that are immutable.

The downsides of this model are the vast number of proxies, many additional network connections, and a complex redirection logic to feed network traffic into the proxies. On top of that, there are also limitations in what type of network traffic can be redirected to a layer 4 proxy. Proxies are limited in what network protocols they can support.

## A history of connectivity moving into the Kernel

Providing secure and reliable connectivity between applications has been the responsibility of the operating system for decades. Some of you may remember [TCP Wrappers](https://en.wikipedia.org/wiki/TCP_Wrappers) and tcpd from earlier Unix and Linux days. It could be considered the original sidecar. tcpd allowed users to transparently add logging, access control, host name verification, and spoof protection to applications without modifying them. It used libwrap, and, in an interesting parallel to the service mesh story, this same library was what applications previously linked with to provide these capabilities. What tcpd brought to the table was the ability to transparently add the functionality to existing applications without modifying them. Eventually, all of this functionality found its way into Linux itself and became available to all applications in a more efficient and powerful manner. Today, this has evolved to what we know as iptables.

However, iptables is clearly not suitable to solve the connectivity, security, and observability requirements of modern applications as it operates exclusively on the network level and lacks any understanding of the application protocol layer. Naturally, the path of least resistance was to go back to the library model, then the sidecar model. Now we are at the point where it makes sense to support this natively in the operating system for optimal transparency, efficiency, and security.

![Evolution of Service Mesh](/media/Evolution_of_Service_Mesh.png)

What used to be connection logging back in the days of tcpd is now tracing. Access control on IP level has evolved into authorization on the application protocol level, for example with JWT. Host name verification has been replaced with much stronger authentication such as mutual TLS. Network load-balancing has been extended with L7 traffic management capabilities. HTTP retries are the new TCP retransmissions. What used to be solved with blackhole routes is called circuit breaking today. None are fundamentally new but the required context and control have evolved.

## Extending the Kernel Namespace Concept

The Linux kernel already has a concept to share common functionality and makes it available to many applications running on the system. This concept is called namespaces and it forms the foundation of container technology as we know it today. Namespaces (the kernel kind, not the Kubernetes version) exist for a variety of abstractions including file systems, user management, mounted devices, processes, network, and several more. This is what allows individual containers to be presented with a different view of the file system, a different set of users, and what allows multiple containers to bind to the same network port on a single host. This concept is expanded with the help of cgroups to apply resource management and prioritization for resources like CPU, memory, and the network. From the perspective of cloud native application developers, cgroups and resources are tightly integrated into the concept we know as “containers”.

It’s only logical that if we consider service mesh as a responsibility of the operating system, then it must conform to and integrate with the concept of namespaces and cgroups. This will look something like this:

![Service Mesh Namespaces](/media/Service_Mesh_Namespaces.png)

Unsurprisingly, this looks very natural and is probably what most users expect from a simplicity perspective. Applications remain unchanged, they continue to use sockets to communicate as they always have. The desirable service mesh functionality is provided transparently as part of Linux. It's just there, like TCP is there today.

### The Cost of Sidecar Injection

If we look closer into the sidecar model, we notice that it is actually trying to emulate this model. The application continues to use sockets and everything gets stuffed into a network namespace of the Linux kernel. However, it is more complex than it looks, many additional steps are required to transparently inject the sidecar proxy:

![Cost of sidecar injection](/media/Cost_of_sidecar_injection.png)

This additional complexity comes at a significant cost in terms of latency and additional resource consumption. Early benchmarks indicate that this can impact latency up to 3-4x and a significant amount of additional memory is required for all the proxies. We'll look into both later on in this post as we compare it to an eBPF-based model.