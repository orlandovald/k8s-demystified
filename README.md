# k8s-demystified

Companion repo for my Kubernetes Demystified talk

## Pre-requisites

To run the examples you'll need the below installed in your machine.

- [kubeclt](https://kubernetes.io/docs/reference/kubectl/) - Kubernetes CLI to interact with a cluster
- [k3d](https://k3d.io/) - to run your local Kubernetes cluster in docker

## Set up

Create the cluster with the below command,

```sh
k3d cluster create --config ./cluster/k8s-demystified.yaml
```

A k3d cluster named `k8s-demystified` (as specified in the
[./cluster/k8s-demystified.yaml](./cluster/k8s-demystified.yaml) file) will be created and automatically
configured in your local `~/.kube/config` file. You can verify your kubectl is
pointing to your local cluster with the below command.

```sh
$ kubectl config current-context

# you can run this at any time to point to your new cluster
$ kubectl config use-context k3d-k8s-demystified

# let's take a look at the Nodes in your cluster
$ kubectl get nodes
```

Finally, let's create a namespace where we can start creating resources,

```sh
# let's create a couple of namespaces
kubectl create namespace redis
kubectl create namespace k8s-demystified

# point to your namespace for all subsequent kubectl commands
kubectl config set-context --current --namespace=k8s-demystified
```

This finishes the set up. You can continue with the [tutorial](tutorial.md)

## Teardown

To delete your local cluster, including all created resources, run the below,

```sh
k3d cluster delete k8s-demystified
```
