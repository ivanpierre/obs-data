# External etcd topology

An HA [[Cluster]] with external [[etcd]] is a [topology](https://en.wikipedia.org/wiki/Network_topology) where the distributed data storage [[Cluster]] provided by [[etcd]] is external to the [[Cluster]] formed by the nodes that run control plane components.

Like the [[Stacked etcd topology]], each control plane node in an [[External etcd topology]] runs an instance of the `kube-apiserver`, `kube-scheduler`, and `kube-controller-manager`. And the `kube-apiserver` is exposed to worker nodes using a load balancer. However, [[etcd]] members run on separate hosts, and each [[etcd]] host communicates with the `kube-apiserver` of each control plane node.

This topology decouples the control plane and [[etcd]] member. It therefore provides an HA setup where losing a control plane instance or an [[etcd]] member has less impact and does not affect the [[Cluster]] redundancy as much as the stacked HA topology.

## additional hardware
**However, this topology requires twice the number of hosts as the stacked HA topology. A minimum of three hosts for control plane nodes and three hosts for [[etcd]] nodes are required for an HA [[Cluster]] with this topology.
**
![External etcd topology](https://d33wubrfki0l68.cloudfront.net/ad49fffce42d5a35ae0d0cc1186b97209d86b99c/5a6ae/images/kubeadm/kubeadm-ha-topology-external-etcd.svg)