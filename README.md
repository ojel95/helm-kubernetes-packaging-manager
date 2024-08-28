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