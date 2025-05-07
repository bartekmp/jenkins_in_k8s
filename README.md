# jenkins_in_k8s

This repo is a product of exercising k8s deployment on a single node local cluster.

This set of YAML files can be used to spawn a single-replica Jenkins instance on your own cluster, with a dedicated persistent volume storage, for accessing the data from the host.

## How to deploy
1. Clone the repository.
2. Ensure having `kubectl` tool installed on the host.
3. Ensure the Kubernetes cluster is up and running.
4. Enter the repo directory and deploy everything with:
```bash
$ kubectl apply -f .
```
5. Check the deployment:
```bash
$ kubectl -n devops describe deployment
```
6. Access it using your cluster's address: `http://<clusterIP>:32000` and proceed with the setup.

## What it does
Each of the YAML files has additional comments for better understanding of what's going on.

Applying it creates a dedicated namespace `devops` ([namespace.yaml](namespace.yaml)), cluster account, role and binding ([account.yaml](account.yaml)), on-host volume storage under `/var/jenkins-pv` ([volume.yaml](volume.yaml)), service and HTTP port exposure ([service.yaml](service.yaml)) and the Jenkins unprivileged container deployment in a single pod ([deployment.yaml](deployment.yaml)).

## Admin password
The web interface will ask you for the Admin password at the first launch. It is printed to the pod's log:
```bash
$ kubectl -n devops get pods
<the pod ID>
$ kubectl -n devops logs <the pod ID>
<...>
Jenkins initial setup is required. An admin user has been created and a password generated.
Please use the following password to proceed to installation:

<your_jenkins_password>
```