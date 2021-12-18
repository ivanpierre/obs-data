# k3OS

k3OS is a Linux distribution designed to remove as much OS maintenance as possible in a Kubernetes cluster. It is specifically designed to only have what is needed to run [k3s](https://github.com/rancher/k3s). Additionally the OS is designed to be managed by `kubectl` once a cluster is bootstrapped. Nodes only need to join a cluster and then all aspects of the OS can be managed from Kubernetes. Both k3OS and k3s upgrades are handled by the k3OS operator.

- [GitHub - rancher/k3os: Purpose-built OS for Kubernetes, fully managed by Kubernetes.](https://github.com/rancher/k3os)
- [The Kubernetes Operating System - k3OS](https://k3os.io/)