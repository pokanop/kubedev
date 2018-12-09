## Overview

This is a step by step guide to help get a Kubernetes cluster running locally with all the tools needed for deploying and monitoring.

Once completed, ideally you should be able to build your own microservices and deploy locally using CI/CD. Additionally, you should be able see metrics from you applications as well as access your cluster.

For more information on each technology, it is recommended to learn about them separately and in depth as this guide was made with a cursory understanding and may not choose the best settings or omit details, but it works for me:tm:.

---

## Motivation

There are many technologies out there today and this is to help me remember how to get my dev environment set up and functional for deploying services locally. It does not intend to provide any guidance on best practices or inform on _the right way_ to do things. It's a culmination of my learnings on the internets :blush:

The goal here is to run a bunch of services locally that are basically home brewed goods. To deploy on the :cloud: you'll have to take the extra steps in figuring out how to do that. I don't imagine it'd be too difficult but not a goal for this guide.

I feel though this is a nice baseline _if you set everything up_ where most things afterward can be additive. Hopefully it may have something useful for folks other than me.

Lastly, if something is incorrect then apologies in advance but surely someone must have once said:

> It's easier to ask for forgiveness than for permission

Feel free to post pull requests with fixes :floppy_disk:

---

## Technologies

This guide uses the following technologies and tools:

- [Docker](#docker)
- [Kubernetes](#kubernetes)
- [Minikube](#minikube)
- [Helm](#helm)
- [Kubeapps](#kubeapps)
- [Docker Registry](#docker-registry)
- [Chartmuseum](#chartmuseum)
- [Prometheus](#prometheus)
- [Grafana](#grafana)
- [Jenkins](#jenkins)

### Docker

[![Docker Image](images/docker.png)](https://www.docker.com/)

Docker is simply a technology that lets you run things in containers. It provides the benefit of bundling up all the libraries and dependencies and running the same way in dev or in production. You can find more detailed definitions [here](https://opensource.com/resources/what-docker).

All the apps/services that we'll run will be containerized for ease of deployment. There are other tools built on top of Docker that help manage images and deployment.

### Kubernetes

[![Kubernetes Image](images/kubernetes.png)](https://kubernetes.io/)

Kubernetes is a set of services that help orchestrate containers. It's a state management system that you can tell that you want to run `n` instances of a service and how to expose it through load balancers and ingresses, etc.

It's a pretty complicated architecture but boils down to nodes, deployments, ingresses, services, and pods among other things. These things define the desired **state** in a way and the Kubernetes system figures out how to make the magic happen in one or many clusters.

Kubernetes comes with `kubectl` which is the command line interface to access your running pods, services etc. and managing deployments. This is a handy tool but is cumbersome if working with large scale deployments and complex dependencies.

### Minikube

[![Minikube Image](images/minikube.png)](https://github.com/kubernetes/minikube)

Since Kubernetes is many services running together to form a cluster, it's not trivial to run independently but is usually deployed on the cloud as a managed services through providers like [AWS](https://aws.amazon.com/) or [GCP](https://cloud.google.com/gcp).

Minikube is an awesome way to run a single Kubernetes cluster on your local machine. It's likely not recommended to use this for production but that's what I'm going to do because I don't have :moneybag:.

### Helm

[![Helm Image](images/helm.png)](https://www.helm.sh/)

Helm is considered a package manager for Kubernetes. It uses the concept of a `Chart.yml` file to define an application, and templates that are rendered based on various values provided.

It supports versioning deployments and rollbacks which is neat :squirrel: and saves from having to manually run `kubectl` repeatedly for a bunch of things that can be automated.

There is a service named Tiller that runs in your Kubernetes cluster that presumably does housekeeping and talks to the client interface `helm`.

### Kubeapps

[![Kubeapps Image](images/kubeapps.png)](https://github.com/kubeapps/kubeapps)

I discovered Kubeapps as a nice web based GUI to find and deploy Helm charts in your cluster. Not necessarily an automated or CI like solution, but for getting things set up the first time it has been pretty useful for me.

Kubeapps connects to Helm and various chart repositories (including custom ones which we'll do) used to deploy in the cluster.

### Docker Registry

[![Docker Image](images/docker.png)](https://docs.docker.com/registry/)

Docker has a central repository where it stores images at [Docker Hub](https://hub.docker.com/). Most of the popular applications are available there and serve as the base for most custom images that we use. However, if you want to run your own registry you can do exactly that with Docker Registry.

It's nice to keep a registry for your organization on-site or in the cloud so your images are private.

When you _dockerize_ an application you use a `Dockerfile` and typically you build an image from that and upload it to the registry so you can deploy elsewhere.

### Chartmuseum

[![Chartmuseum Image](images/chartmuseum.png)](https://github.com/helm/chartmuseum)

Like Docker Registry, Helm charts also need a home so again there is a centralized repository for all public charts [here](https://github.com/helm/charts).  But you can run your own repository using Chartmuseum for private charts as well.

### Prometheus

[![Prometheus Image](images/prometheus.png)](https://prometheus.io/)

Prometheus provides metrics collection infrastructure and alerting as well. Paired with a visualization tool like Grafana and it helps you diagnose issues and monitor your services.

It works by expecting services to expose and `HTTP` endpoint at `/metrics` which it will periodically scrape for data. In Kubernetes you can use annotations for the service that Prometheus is aware of to specify that it supports this.

### Grafana

[![Grafana Image](images/grafana.png)](https://grafana.com/)

Grafana is a dashboard for visualizing metrics with graphs and insights on how your services are operating. It's valuable for monitoring that everything is running smoothly.

### Jenkins

[![Jenkins Image](images/jenkins.png)](https://jenkins.io/)

No ecosystem is complete without a good CI system and Jenkins provides just that. This is the final piece that will take all of the applications and bind them together with :heart:. It will take the dockerized service, build it, and deploy it in your Kubernetes cluster automagically from your chart repository and Docker registry.

---

## Guides

- [Setting up Docker, Minikube, and Helm](docs/setup-1.md)
- [Setting up Kubeapps, Prometheus, and Grafana](docs/setup-2.md)
- [Setting up Docker Registry, Chartmuseum, and Jenkins](docs/setup-3.md)
- [Building and deploying a microservice](docs/microservice.md)
