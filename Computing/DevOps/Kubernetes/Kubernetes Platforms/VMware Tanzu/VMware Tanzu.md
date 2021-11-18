# Tanzu Community Edition

VMware Tanzu Community Edition is a full-featured, easy-to-manage Kubernetes platform for learners and users, especially those working in small-scale or preproduction environments

![main](media/TCE-logo.png)

VMware Tanzu enables you to build, run and manage modern apps on any cloud-and continuously deliver value to customers. With Vmware Tanzu, you can simplify multi-cloud operations and free developers to move faster with easy access to the right resources. VMware Tanzu enables development and operations teams to work together in new ways that deliver transformative business results.

Vmware has released a community version of tanzu that can be downloaded from the official [site](https://tanzucommunityedition.io/) and with which you can play around with this amazing product.

VMware Tanzu Community Edition is a full-featured, easy to manage Kubernetes platform for learners and users. It is a freely available, community supported, open source distribution of VMware Tanzu that can be installed and configured in minutes on your local workstation or your favorite cloud.

Tanzu Community Edition consists of the Tanzu CLI and a select set of plugins. You will install Tanzu Community Edition on your local machine and then use the Tanzu CLI on your local machine to deploy a cluster to your chosen target platform.

I am going to show you the procedure to install TCE in MAC but this not limited to this OS only. It can be installed in Linux, Mac & Windows.

![main](media/prerequisitesmac.png)

Run the following in your terminal:

```
brew install vmware-tanzu/tanzu/tanzu-community-edition
```

Run the post install configuration script. Note the output of the `brew install` step for the correct location of the configure script:

```
{HOMEBREW-INSTALL-LOCATION}/v0.9.1/libexec/configure-tce.sh
```

This puts all the Tanzu plugins in the correct location. The first time you run the tanzu command the installed plugins and plugin repositories are initialized. This action might take a minute.

## Creating Clusters

With Tanzu Community Edition you can deploy your workload clusters in AWS, Azure, VSphere o Docker.

We are going to play around with deploying our workload to Docker locally on my Mac.

Before start it is recommended to stop all existing containers. Run following command in your terminal:

```
docker kill $(docker ps -q)
```

It is also recommended to prune all existing containers, volumes and images by running following command:

```
 docker system prune -a --volumes
```

I can check in my docker dashboard that nothing is running and everything was cleaned out.

![main](media/docker.png)

Now we are ready to initialize the Tanzu Community Edition installer interface.

```
tanzu management-cluster create --ui
```

This command will launch a web interface locally in your computer like this:

![main](media/tce-ui.png)

From there we can choose where we want to deploy our workload cluster. This time I am going to deploy using Docker.

You will provide the name of the cluster:

![main](media/main.png)

And you wil also select the Kubernetes network settings:

![main](media/k8s-networking.png)

At the end you will review the configuration and then you can click on deploy the managment cluster or optionally you can deploy using the CLI command equivalent provided by the tool as you can see in the following image:

![main](media/review-config.png)

Let’s inspect what is the content of the YAML file that is going to be used for the dpeloyment of the cluster:

```
CLUSTER_CIDR: 100.96.0.0/11
CLUSTER_NAME: blog-demo
ENABLE_MHC: "false"
IDENTITY_MANAGEMENT_TYPE: none
INFRASTRUCTURE_PROVIDER: docker
LDAP_BIND_DN: ""
LDAP_BIND_PASSWORD: ""
LDAP_GROUP_SEARCH_BASE_DN: ""
LDAP_GROUP_SEARCH_FILTER: ""
LDAP_GROUP_SEARCH_GROUP_ATTRIBUTE: ""
LDAP_GROUP_SEARCH_NAME_ATTRIBUTE: cn
LDAP_GROUP_SEARCH_USER_ATTRIBUTE: DN
LDAP_HOST: ""
LDAP_ROOT_CA_DATA_B64: ""
LDAP_USER_SEARCH_BASE_DN: ""
LDAP_USER_SEARCH_FILTER: ""
LDAP_USER_SEARCH_NAME_ATTRIBUTE: ""
LDAP_USER_SEARCH_USERNAME: userPrincipalName
OIDC_IDENTITY_PROVIDER_CLIENT_ID: ""
OIDC_IDENTITY_PROVIDER_CLIENT_SECRET: ""
OIDC_IDENTITY_PROVIDER_GROUPS_CLAIM: ""
OIDC_IDENTITY_PROVIDER_ISSUER_URL: ""
OIDC_IDENTITY_PROVIDER_NAME: ""
OIDC_IDENTITY_PROVIDER_SCOPES: ""
OIDC_IDENTITY_PROVIDER_USERNAME_CLAIM: ""
OS_ARCH: ""
OS_NAME: ""
OS_VERSION: ""
SERVICE_CIDR: 100.64.0.0/13
TKG_HTTP_PROXY_ENABLED: "false"
```

This is insteresting enough that we can think of automate these deployments using a CI/CD tool. Let’s try this in another blog post 😄 .

Let’s dive in the managed cluster we are deploying to Docker.

![main](media/bootstrap-cluster-create.png)

Clusters that are deployed and managed using tanzu cluster command(s) are known as managed clusters. These clusters are deployed and managed by a Tanzu management cluster (originally deployed using tanzu management-cluster. This is the primary deployment model for clusters in the Tanzu ecosystem and is recommended for production scenarios. To bootstrap managed clusters, you first need a management cluster. This is done using the tanzu management-cluster create command. When running this command, a bootstrap cluster is created locally and is used to then create the management cluster. The following diagram shows this flow.

Once the management cluster has been created, the bootstrap cluster will perform a move (aka pivot) of all management objects to the management cluster. From this point forward, the management cluster is responsible for managing itself and any new clusters you create. These new clusters, managed by the management cluster, are referred to as workload clusters. The following diagram shows this relationship end-to-end.

![main](media/management-cluster-flow.png)

Going back to our installation procedure. This is completed after few minutes and we can now close the web browser:

![main](media/installation-completed.png)

Validate the management cluster started:

```
tanzu management-cluster get
```

You should get an output like this:

![main](media/manage-cluster-cli.png)

Let’s also check the containers that are part of my Managed CLuster:

![main](media/manage-cluster-docker.png)

Capture the management cluster’s kubeconfig and take note of the command for accessing the cluster in the message, as you will use this for setting the context in the next step. In our case:

```
tanzu management-cluster kubeconfig get blog-demo --admin
```

You should get an output like this:

```
Credentials of cluster 'blog-demo' have been saved 
You can now access the cluster by running 'kubectl config use-context blog-demo-admin@blog-demo'
```

Set your kubectl context to the management cluster. In our case:

```
kubectl config use-context blog-demo-admin@blog-demo
```

Validate you can access the management cluster’s API server.

You will see output similar to:

```
NAME                              STATUS   ROLES                  AGE   VERSION
blog-demo-control-plane-ljttm     Ready    control-plane,master   10h   v1.21.2+vmware.1-360497810732255795
blog-demo-md-0-6877dfbdbf-4fmxw   Ready    <none>                 10h   v1.21.2+vmware.1-360497810732255795
```

## Workload cluster

Now that we have a Managed Cluster we can create any workload clusters where ideally we will deploy our applications and instrument our environments for our apps.

Let’s create one (develop) workload cluster with plan `dev`

```
tanzu cluster create develop --plan dev
```

Validate the cluster starts successfully.

Capture the workload cluster’s kubeconfig.

```
tanzu cluster kubeconfig get develop --admin
```

Set your kubectl context accordingly.

```
kubectl config use-context develop-admin@develop
```

Verify you can see pods in the cluster.

```
kubectl get pods --all-namespaces
```

You now have local clusters running on Docker. Let’s deploy a Nginx pod in our workload develop cluster.

We can use the same managed cluster as a control plane to deploy our Kubernetes clusters in AWS, Azure & VSphere as well.

Which is very versatil with Tanzu is you can easy install any package as a plugin provided by Tanzu. Specifically for Tanzu Community Edition these are the packages available that you can deploy into your workload cluster with one command:

![main](media/main-2.png)

## Resources

If you want to learn more about Vmware Tanzu and Kubernetes please check following resources:

-   [KubeAcademy](https://kube.academy/) is a free, product-agnostic Kubernetes and cloud native technology education program built by a team of expert instructors. Courses comprise a series of short video lessons and dive into topics for skill levels ranging from beginner to advanced.
    
-   Every week, [TGIK](https://www.youtube.com/playlist?list=PL7bmigfV0EqQzxcNpmcdTJ9eFRPBe-iZa) hosts live-stream their dive into new Kubernetes tools and concepts. Join every Friday at 1:00 PM PT to follow along and learn.
    
-   Visit the [Tanzu Developer Center](https://tanzu.vmware.com/developer/) to learn more about cloud native technologies and practices. You’ll find informative articles, guides, videos, samples, workshops, and more on topics that matter to you:
    
-   [Getting Started with Tanzu Community Edition](https://tanzucommunityedition.io/docs/latest/getting-started/)