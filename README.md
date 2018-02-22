# Getting started with the Lightbend Orchestration for Kubernetes on the IBM Cloud

Lightbend describe their new [Lightbend Orchestration for Kubernetes](https://developer.lightbend.com/docs/reactive-platform-tooling/latest/overview.html) as "... a developer-centric suite of tools that helps you deploy Reactive Platform applications to Kubernetes. It provides an easy way to create Docker images for your applications and introduces an automated process for generating Kubernetes resource files for you from those images."

As a Reactive Application developer you may choose to run Kubernetes on your development machine using Minikube. As your application grows and the number of microservices increases, you may choose to offload Kubernetes to a cloud-based solution. The Lightbend Orchestration for Kubernetes documentation points to [the IBM Cloud Container Service](https://developer.lightbend.com/docs/reactive-platform-tooling/latest/cluster-setup.html) as an alternative to Minikube. In this article, I'll describe how to make that switch, while using the [Chirper](https://github.com/longshorej/lagom-java-chirper-tooling-example) Twitter-like sample application - a Lagom based example used to showcase the Reactive applications running on Kubernetes.

Kubernetes is fundamentally about orchestrating the Docker containers. Docker containers are created from Docker images and those are held in a Docker registry. When you build Docker images on your laptop they are stored locally. Minikube uses this local store to fetch images and create containers. When you run Docker containers in a remote Kubernetes cluster, a remote Docker registry is needed. IBM Cloud provides one of those in the [Container Registry](https://www.ibm.com/cloud/container-registry) and, as you'd expect the Container Service (Kubernetes) and the Container Registry are well integrated.

## Register with IBM Cloud
To use the IBM Cloud Container Service you need to register with IBM Cloud. You will be able to create a free Kubernetes cluster with a single [Node](https://kubernetes.io/docs/concepts/architecture/nodes/), as long as you [provide some payment details](https://console.bluemix.net/account/billing). Presumably to put off opportunistic Bitcoin miners. If you already have a Lite account, then you will need to add payment details to unlock your free cluster. If not, then use the [Pay as you go option](https://www.ibm.com/cloud/pricing) from the start.

## Create your free cluster
IBM Cloud provides a single command line tool: `bx` for controlling the resources and services available to you. Using the Container Service, you can create a Kubernetes cluster through using `bx` but you may find it easier, at least the first time, to create your free cluster through the [web interface](https://console.bluemix.net/containers-kubernetes/clusters).

A short time after you've initiated the cluster creation, the web interface shows instructions for downloading the `bx` CLI, installing the container-service plugin into it, logging into your account ... all the way to configuring `kubectl` with the cluster and showing the Kubernetes Dashboard for the cluster you've created. Like this:

![Kubernetes Dashboard](images/KubeDash.png)

## Somewhere to put Docker images
When you run you microservices on Kubernetes, Kubernetes deploys Docker containers made from Docker images which it retrieves from a Docker registry. Your IBM Cloud account has a Docker registry associated with it, called the 'Container Registry' and to start with this registry is empty.

The [Container Registry Quick Start](https://console.bluemix.net/containers-kubernetes/registry/start) gives instructions, for installing the container-registry plugin to the `bx` CLI and how to create a namespace which can receive images pushed to it. This will come in useful later when we use Lightbend Orchestration for Kubernetes to push the Chirper application images to the registry.

## Developing Chirper with the IBM Cloud
You may have already followed [the instructions for setting up the Chirper example](https://github.com/longshorej/lagom-java-chirper-tooling-example/blob/master/README.md) which includes instructions specific to Minikube. The set of prequisites and steps are similar for deploying to IBM Cloud:

Install the following programs on you system:

* [Docker](https://store.docker.com/search?type=edition&offering=community)
* [Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md)
* [SBT](http://www.scala-sbt.org/)
* [reactive-cli](https://developer.lightbend.com/docs/reactive-platform-tooling/latest/#install-the-cli)

Now clone the Chirper sample referred to by the Lightbend Orchestration for Kubernetes project:

```
$ git clone https://github.com/longshorej/lagom-java-chirper-tooling-example.git chirper
$ cd chirper
```

Add a namespace to the Container Registry to hold the images the Chirper build process will create:

```
$ bx plugin install container-registry -r Bluemix
$ bx cr namespace-add chirper
Adding namespace 'chirper'...
Successfully added namespace 'chirper'
```

When using Minikube you invoke the sbt-native-packager using `sbt docker:publishLocal` but we're going to switch to publish to the Container Registry. For that we need to give `sbt` provide the registry location and namespace to which it will publish the images. You may already know the location of the Container Registry is - there is one in each IBM Cloud region. It's the 'Container Registry' line output when you execute `bx cr info`. For the namespace, we're going to use 'chirper'.

Add the following to the `build.sbt` file to the end of the definition of the `project` method around line 122:

```
  .settings(
    dockerRepository := Some("registry.eu-gb.bluemix.net/chirper")
  )
```

Note: I'm in the UK so my registry hostname has `eu-gb` in its name. Yours may differ.

Now, when you execute:

```
$ bx cr login
$ sbt docker:publish
```

the first command will redirect the local Docker engine to the Container Registry, and the second command will build the code and publish the images there.

You can check the images have uploaded by viewing the [IBM Cloud console page your private registry](https://console.bluemix.net/containers-kubernetes/registry/private). Which should look similar to:

![Container Registry after publishing images](images/ContainerReg.png)


