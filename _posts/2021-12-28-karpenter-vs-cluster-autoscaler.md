---
layout: post
title: "Kubernetes Cluster Autoscaling: Karpenter Vs Cluster Autoscaler"
date: 2021-12-28
author: Virendra Singh Bhalothia
tags: k8s cluster-autoscaler aws containers karpenter
category: tutorial
excerpt: "In this blog we will explore how Karpenter works as compared to Cluster Autoscaler."
---

## Kubernetes Cluster Autoscaling: Karpenter Vs Cluster Autoscaler
### AWS Announcement at re:Invent 2021
AWS has [released](https://aws.amazon.com/blogs/aws/introducing-karpenter-an-open-source-high-performance-kubernetes-cluster-autoscaler/) the GA version of Karpenter at **re:Invent 2021**. 

:tada:


[Karpenter](https://karpenter.sh/) is an open-source, flexible, high-performance Kubernetes cluster autoscaler built with AWS.

This means it's officialy ready for production workloads as per AWS. However, it's been discussed since almost an year. Some of the [conference talks](https://github.com/aws/karpenter#talks) can be found on the Karpenter's [Github repo](https://github.com/aws/karpenter).


The official blog announcement promises support for K8s clusters rynning in **any** environment:

>Karpenter is an open-source project licensed under the Apache License 2.0. It is designed to work with any Kubernetes cluster running in any environment, including all major cloud providers and on-premises environments.

Currently, it **only supports AWS Cloud**, but do keep an eye on the [Karpenter project roadmap](https://github.com/aws/karpenter/projects/3) if you use other underlying cloud providers or on premises datacenters. 


### Kubernetes Autoscaling Capabilities

Alright! So, before we get started with comparing Cluster Autoscaler with Karpenter; let's quickly go over the autoscaling capabilities Kubernetes offers.

> Kubernetes enables autoscaling at the node level as well as at the pod level. These two are different but fundamentally connected layers of Kubernetes architecture.

#### Pod based autoscaling with:
- [Horizontal Pod Autoscaler](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/)
- [Vertical Pod Autoscaler](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler)

#### Node based autoscaling with:
- [Cluster Autoscaler](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler)
- [Karpenter](https://karpenter.sh/docs/getting-started/)



:bulb: 

**NOTE**: This post will only explore the aforementioned Kubernetes Cluster Autoscalers.

### Kubernetes Cluster Autoscalers: What has changed with Karpenter?

[Cluster Autoscaler](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler#introduction) is a Kubernetes tool that increases or decreases the size of a Kubernetes cluster (by adding or removing nodes), based on the presence of pending pods and node utilization metrics. Cluster Autoscaler. It automatically adjusts the size of the Kubernetes cluster when one of the following conditions is true:

- there are pods that failed to run in the cluster due to insufficient resources.
- there are nodes in the cluster that have been underutilized for an extended period of time and their pods can be placed on other existing nodes.

[Karpenter](https://github.com/aws/karpenter#readme) automatically provisions new nodes in response to unschedulable pods. Karpenter does this by observing events within the Kubernetes cluster, and then sending commands to the underlying cloud provider. Karpenter works by:

- Watching for pods that the Kubernetes scheduler has marked as unschedulable
- Evaluating scheduling constraints (resource requests, nodeselectors, affinities, tolerations, and topology spread constraints) requested by the pods
- Provisioning nodes that meet the requirements of the pods
- Scheduling the pods to run on the new nodes
- Removing the nodes when the nodes are no longer needed

#### Architecture
Cluster Autoscaler watches for pods that fail to schedule and for nodes that are underutilized. It then simulates the addition or removal of nodes before applying the change to your cluster. The AWS Cloud Provider implementation within Cluster Autoscaler controls the `.DesiredReplicas` field of your EC2 Auto Scaling Groups.

![Cluster Autoscaler Architecture](/assets/img/blog/autoscalers/cluster_autoscaler.png)

-- TODO 
- IAM policies
- Resources creation
- Autoscaling




## Let's get started with Karpenter

Pre-requisites
- cli, tf, helm etc
- tagging

Example workload with Cluster autoscaler and karpenter

What happens to the AMI and kubelet extra args? 
