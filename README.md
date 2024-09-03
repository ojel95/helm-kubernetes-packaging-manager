# helm-kubernetes-packaging-manager

This is the learning material of a course to learn the fundamentals of Helm with Kubernets. Here you can find exercises and notes made during the course.

## Issues helm attacks

- **Consistency**: When using IaC, if you made changes in your k8s yaml configuration files but alse make some changes using the kubectl
directly in the cluster, you will have consistency issues.

- **Revision history**: Kubernetes as it is, doesn't keep a version history of the cluster configuration. That is, if you made
changes in multiple resources files like the Service, ConfigMap, Ingress and so on, you have to make a redeployment of the
whole thing, and if at some point you want to go back to a particular version there is no way.

## What is Helm

Helm is a packages manager like apt but in the world of kubernetes.

Works with charts, and the charts are like packages in the world of helm.

The charts have all the template files and the configuration required to create kubernetes resources.

You can use only one command to install, upgrade, rollback and uninstall releases that lives in internal/external repositories.
These releases are the charts that takes all the templates and values and creates the resources that are send to the Kubernetes
cluster.

## Why Helm

- Simplifies the Kubernetes deployment process by abstracting all the complexity.
- You can use public charts that have been already created by the experts on the app component like MongoDB, so you don't have
to have a deep knowledge of that application in order to install it in your Kubernetes cluster.
- It create revision history with each upgrade with resources changes.
- Dynamic configuration: With Kubernetes directly, the yaml files are static. Don't accept any values. And we have to repeat the
same configuration in multiple projects. Helm give us this files as templates that have placeholders for parameters. Using a values.yaml
file we can pass parameters to this template.
- When we use packaging managers, we do all the installations and upgrades through the packaging manager. We don't have to tweak anything
directly. Same with Helm when we use Helm to do Kubernetes. Installations and upgrades will not go to the Kubernetes cluster and change
the resources. If we want to do an upgrade, we simply use the helm upgrade command so we can always be sure that whatever we have
locally in the chart is always consistent with what is deployed in the Kubernetes cluster.
- Intellegent deployments: It knows the order in which Kubernetes resources should be created, and it will automatically do it
for us.
- Security:  Helm has inbuilt support to ensure that the charts that are downloaded from the central repositories are secured.
The charts can be signed using cryptography and hashes can be generated. And when we install these charts, we're pulling them
from the central repos. Helm will verify that these charts are really from the source we are expecting and they are not tweaked
by any hacker.

## Key concepts

### Repositories

Are where the charts are store and then pulled.

One of the most popular public respositories is [Bitnami]("https://bitnami.com/stacks/helm"). There you can find official charts for
different technologies.

### Kube config file

The env variable KUBECONFIG is the one that point to config file. You can export new values to change the file used.

## Commands

### Repository commands

list your added repos: `helm repo list`

Add a new repo: `helm repo add bitnami https://charts.bitnami.com/bitnami`

Remove a repo: `helm repo remove bitnami`

Update the repo: `helm repo update`

**Search the repository**:

Search charts in the repo: `helm search repo mysql`

Search charts with all its versions: `helm search repo database --versions`

### Installation

Install a package: `helm install mydb bitnami/mysql`

- The installation name should be unique per namespace. You can add `-n namespace_name` to install it in a different namespace
- You can set custom configuration or values during installation (NOT RECOMMENDED) adding `--set auth.rootPassword=test1234` for example.
- The recommended way is to use a values config file. Adding `--values path-to/values.yaml`

To check the installation status: `helm status mydb`

To list the installed packages: `helm list`

  - You can add `-n namespace_name` to list in other namespace than default.

### Uninstallation

Unistall installed package: `helm uninstall mydb`

  - Same as installation you can specify the namespace using `-n namespace_name`

Uninstall keeping the history: `helm unistall mydb --keep-history`

## Upgrade

Here you can check the installation output messages in order the define if you need specific value. In the case of
the previous examples with mysql you need to set the rootPassword value. You can do it with --set which is not the
recommended way or with the values file.

Helm is intelligent to upgrade only the resources with changes and you will see that the REVISION numbers is updated.

``` bash
helm upgrade --namespace default mysql-release bitnami/mysql --values ./path/to/values.yaml
```

Here you have to provide the same values used in installation or you can use:

``` bash
helm upgrade --namespace default mysql-release bitnami/mysql --reuse-values
```

When you do an upgrade, helm will create a new secret in the cluster and there is where it stores the release information. Check it with: `kubectl get secrets`
