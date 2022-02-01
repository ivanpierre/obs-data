# etcd

## What is etcd?

**etcd** is a strongly consistent, distributed key-value store that provides a reliable way to store data that needs to be accessed by a distributed system or [[Computing/DevOps/Kubernetes/Cluster/Cluster]] of machines. It gracefully handles leader elections during network partitions and can tolerate machine failure, even in the leader node.

## Features
### Simple interface

Read and write values using standard HTTP tools, such as curl

![Simple interface feature icon](https://etcd.io/img/interface.svg)

### Key-value storage

Store data in hierarchically organized directories, as in a standard filesystem

![Key-value storage feature icon](https://etcd.io/img/kv.svg)

### Watch for changes

Watch specific keys or directories for changes and react to changes in values

![Watch for changes feature icon](https://etcd.io/img/watch.svg)
