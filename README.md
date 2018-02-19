# rp-tooling-ibm-cloud
Getting started with the Reactive Platform Tooling with IBM Cloud

Lightbend describe their new [Reactive Platform Tooling](https://developer.lightbend.com/docs/reactive-platform-tooling/latest/overview.html) as "... a developer-centric suite of tools that helps you deploy Reactive Platform applications to Kubernetes. It provides an easy way to create Docker images for your applications and introduces an automated process for generating Kubernetes resource files for you from those images."

As a Reactive Application developer you can choose to run Kubernetes on your development machine using Minikube. As the scale of the application increases, Minikube can easily be switched out for a cloud-based Kubernetes solution. In this article, I'll describe how to switch to using the [IBM Cloud Container Service](https://www.ibm.com/cloud/container-service). I'll use the [Chirper](https://github.com/longshorej/lagom-java-chirper-tooling-example) Twitter-like sample application which the Reactive Platform Tooling uses to showcase their integratioin with Kubernetes.
