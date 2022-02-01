# From Borg to [[Computing/DevOps/Kubernetes/Kubernetes]]

According to the abstract of Google's [Borg paper](https://research.google.com/pubs/pub43438.html), published in 2015,

> _"Google's Borg system is a [[Computing/DevOps/Kubernetes/Cluster/Cluster]] manager that runs hundreds of thousands of jobs, from many thousands of different applications, across a number of clusters each with up to tens of thousands of machines"._

For more than a decade, Borg has been Google's secret, running its worldwide containerized workloads in production. Services we use from Google, such as Gmail, Drive, Maps, Docs, etc., they are all serviced using Borg. 

Some of the initial authors of [[Computing/DevOps/Kubernetes/Kubernetes]] were Google employees who have used Borg and developed it in the past. They poured in their valuable knowledge and experience while designing [[Computing/DevOps/Kubernetes/Kubernetes]]. Some of the features/objects of [[Computing/DevOps/Kubernetes/Kubernetes]] that can be traced back to Borg, or to lessons learned from it, are:

-   API servers
-   Pods
-   IP-per-Pod
-   Services
-   Labels.

