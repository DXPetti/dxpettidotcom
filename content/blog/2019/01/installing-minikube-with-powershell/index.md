---
title: "Installing MiniKube with Powershell"
date: "2019-01-30"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "containers"
  - "hyper-v"
  - "k8s"
  - "kubernetes"
  - "powershell"
  - "sysadmin"
  - "technology"
  - "windows"
coverImage: "minikube_cover.jpg"
---

I noted on Twitter the other day that I had mixed feelings on getting a breakthrough on a problem that I had faced a couple of months ago by revisiting said problem with a fresh pair of eyes and a different point of view.

{{< tweet dxpetti 1090220314942832641 >}}

The problem was a pet project of mine born from the sheer audacity of _could I?_ In this case, could I automate the manual process of installing [Minikube](https://kubernetes.io/docs/setup/minikube/); the development environment for Kubernetes platform.

Currently, installing Minikube has a handful of steps that felt rather tedious in the modern age. This is exacerbated by the fact that other Operating Systems such as Linux distros or OSX had package managers handle the installation in a more smooth manner. While there is the option of installing a third party package manager for Windows, I wanted to see how I could approach this problem with the power (!) of Powershell while keeping things as native as possible.

Once I sketched out the problem in Powershell ISE _(I'm only just starting to get on the VS Code bandwagon but I already miss the magic of the CTRL-J snippets)_ , it quickly became apparent that the first major step of installing Hyper-V would require a restart in most, if not all cases and somehow my code needed to live and continue on after said restart.

This is where I got stuck in a bit of despair.

## The magic of Workflows are a lie

It had seemed a straight forward problem, of which there were a couple of ways it could be skinned, but the most appealing seemed to be the use of Workflows in Powershell. Workflows is actually a implementation of the [Windows Workflow Foundation](https://docs.microsoft.com/en-us/dotnet/framework/windows-workflow-foundation/) into the Powershell codebase. It is able to split up code into phases, steps etc... which enables it to run over long periods of time, sustain disruptions such as network outages, power failure etc... and is typically used in .NET applications.

Unfortunately, I banged my head against a brick wall attempting to get workflows to work for this particular endeavour to no avail. Despondent and beaten, I shelved my curiosity and enjoyed the Christmas/New Year festivities.

## New year, new code

Here we are, in 2019 and I came across a fabulous deep dive into the use of [Kubernetes in Secure Environments](https://medium.com/@chrismessiah/docker-and-kubernetes-in-high-security-environments-d851645e8b99) when my curiosity lit up once more and my drive told me I needed to have another go.

After a rethink about what the problem statement is, I focused on what I wanted to achieve rather than the technology on how to achieve it and the solution presented itself in an almost comically simple manner.

My code had already had enough tests in it that it would only perform the steps it needed to perform so all I really needed to do after configuring the Hyper-V feature and rebooting the workstation was to re-run the script. And that folks, is rather easy to do with the right entry into the Registry.

With that history lesson out of the way, I'll cease my infernal yapping and bring forth thy code:

{{< gist dxpetti c20673a232b0f6539f1743101d46b9d0 >}}

It's quite dirty and probably needs another pass but I'd love to hear feedback. Hopefully it gets more people into the world of K8s via the local instance sandbox that is Minikube.
