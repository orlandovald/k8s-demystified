# Kubernetes Dashboard

Dashboard is a web-based Kubernetes user interface. You can use Dashboard to
deploy containerized applications to a Kubernetes cluster, troubleshoot your
containerized application, and manage the cluster resources.

More info in the [Kubernetes Docs](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/)

## Installation

Run the below commands to install the dashboaerd in your local cluster

```sh
# install the dashboard
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
# create the required service account and bind the cluster-admin role
$ kubectl create serviceaccount -n kubernetes-dashboard admin-user
$ kubectl create clusterrolebinding -n kubernetes-dashboard admin-user --clusterrole cluster-admin --serviceaccount=kubernetes-dashboard:admin-user
```

## Log in to the dashboard

```sh
# generate a token to access the web ui
$ kubectl -n kubernetes-dashboard create token admin-user

# Create a proxy to connect to the Kubernetes API from localhost
$ kubectl proxy
```

After the above steps you should be able to access the dashboard by opening
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
in your browser. You'll need to provide the generated token.

> [!WARNING] The sample user created in the tutorial will have administrative
privileges and is for educational purposes only.
