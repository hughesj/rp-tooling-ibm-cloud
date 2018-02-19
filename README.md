# rp-tooling-ibm-cloud
Getting started with the Reactive Platform Tooling with IBM Cloud

Lightbend describe their new [Reactive Platform Tooling](https://developer.lightbend.com/docs/reactive-platform-tooling/latest/overview.html) as "... a developer-centric suite of tools that helps you deploy Reactive Platform applications to Kubernetes. It provides an easy way to create Docker images for your applications and introduces an automated process for generating Kubernetes resource files for you from those images."

As a Reactive Application developer you can choose to run Kubernetes on your development machine using Minikube. As the scale of the application increases, Minikube can easily be switched out for a cloud-based Kubernetes solution. In this article, I'll describe how to switch to using the [IBM Cloud Container Service](https://www.ibm.com/cloud/container-service). I'll use the [Chirper](https://github.com/longshorej/lagom-java-chirper-tooling-example) Twitter-like sample application which the Reactive Platform Tooling uses to showcase their integration with Kubernetes.

Kubernetes is fundamentally about orchestrating the Docker containers. Docker containers are created from Docker images and those are held in a Docker registry. When you build Docker images on your laptop they are stored locally. Minikube uses this local store to fetch images and create containers. When you run Docker containers in a remote Kubernetes cluster, a remote Docker registry is needed. IBM Cloud provides one of those in the [Container Registry](https://www.ibm.com/cloud/container-registry) and, as you'd expect the Container Services (Kubernetes) and the Container Registry are well integrated.

To get started for free (no credit card details required!), [register here](https://console.bluemix.net/registraction/free). Your free 'Lite' account gives you one free Kubernetes cluster to play with. There are, of course [more pricing options](https://www.ibm.com/cloud/pricing).

IBM Cloud provides a single command line tool: `bx` for controlling the resources and services available to you. You can create a Kubernetes cluster through using `bx` but you may find it easier, at least the first time, to create your free cluster through the [web interface](https://console.bluemix.net/containers-kubernetes/clusters).

A short time after you've initiated the cluster creation, the web interface shows instructions for downloading `bx` and `kubectl`, the Kubernetes CLI - which you already have if you've worked through the Reactive Platform Tooling instructions. 

Then you're shown how to configure `kubectl` to retrieve an id-token which you use to log into the Kubernetes Dashboard - a locally running web app using REST API calls to the Kubernetes cluster running on the IBM Cloud. You will see a UI like this:

...


