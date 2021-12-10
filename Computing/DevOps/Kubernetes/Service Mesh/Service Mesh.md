# Service Mesh

Service mesh is a concept describing the requirements of modern cloud native applications with regards to communication, visibility, and security. Current implementations of this concept involve running sidecar proxies in each workload or pod. This is a pretty inefficient way of solving these requirements. In this post, we will look at an alternative to the sidecar model that provides a transparent service mesh with high efficiency at low complexity, with the help of eBPF.

## What is Service Mesh?

With the introduction of distributed applications, additional visibility, connectivity, and security requirements have surfaced. Application components communicate over untrusted networks across cloud and premises boundaries, load-balancing is required to understand application protocols, resiliency is becoming crucial, and security must evolve to a model where sender and receiver can authenticate each other’s identity. In the early days of distributed applications, these requirements were resolved by directly embedding the required logic into the applications. A service mesh extracts these features out of the application and offers them as part of the infrastructure for all applications to use and thus no longer requires to change each application.

![](/media/servicemesh_intro.png)

Looking at the feature set of a service mesh today, it can be summarized as follows:

-   **Resilient Connectivity:** Service to service communication must be possible across boundaries such as clouds, clusters, and premises. Communication must be resilient and fault tolerant.
-   **L7 Traffic Management:** Load balancing, rate limiting, and resiliency must be L7-aware (HTTP, REST, gRPC, WebSocket, …).
-   **Identity-based Security:** Relying on network identifiers to achieve security is no longer sufficient, both the sending and receiving services must be able to authenticate each other based on identities instead of a network identifier.
-   **Observability & Tracing:** Observability in the form of tracing and metrics is critical to understanding, monitoring, and troubleshooting application stability, performance, and availability.
-   **Transparency:** The functionality must be available to applications in a transparent manner, i.e. without requiring to change application code.

In earlier days, service mesh functionality was typically implemented as libraries, requiring each application in the mesh to link to a library written in the application’s language framework. Similar things have happened in the early days of the Internet: back in the day, applications used to ship their own TCP/IP stack! As we’ll discuss in this article, service mesh is evolving to become a kernel responsibility, much as the networking stack did.

![Library-based service mesh model](/media/Library-based_service_mesh_model.png)