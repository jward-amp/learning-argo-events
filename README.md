# Learning Argo Events

A toy repository to experiment with Argo Workflows and Events.

## Setup

First, install the required dependencies and start a local cluster. It is recommended to use
an ephemeral cluster, such as minikube, for this experiment because the event bus doesn't
always clean up correctly. If that happens, simply delete the cluster and start over.

```shell
$ brew install helm kubectl minikube skaffold
$ minikube start --cpus=4 --memory=8gb --driver=docker
```

Next, install Argo [Workflows](https://argo-workflows.readthedocs.io/en/latest/) and
[Events](https://argoproj.github.io/argo-events/) using the community
[Helm charts](https://github.com/argoproj/argo-helm).

```shell
$ helm repo add argo https://argoproj.github.io/argo-helm
$ helm dependency build argo-workflows 
$ helm dependency build argo-events
```

This repo uses [Skaffold](https://skaffold.dev/) to deploy the Helm charts.

```shell
$ kubectl create namespace argo
$ kubectl create namespace argo-events
$ skaffold run -p argo --port-forward
```

Next, deploy the Event Bus from a new terminal window.

```shell
$ kubectl apply -k eventbus
```

Lastly, the experimental resources are all contained in the `samples` directory. This 
repo contains toy examples of an Argo Workflow, Cron Workflow, webhook-based Event Source, 
and a Sensor that launches the Argo Workflow. 

```shell
$ kubectl apply -k samples
```

## Teardown

Remove the installed resources, uninstall the Argo tools, and stop (and delete) minikube.

```shell
$ kubectl delete -k samples
$ kubectl delete -k eventbus
$ skaffold delete -p argo
$ minikube stop
# optional
$ minikube delete
```
