---
title: "Move Quorum Disk Witness to another node"
date: "2013-04-22"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "cluster"
  - "hyper-v"
  - "server-core"
  - "sysadmin"
  - "technology"
  - "windows-server-2008-r2"
---

I cannot say the Failover Cluster Manager is a very intuitive management interface. It is probably close to one of the worst ones I have had the pleasure to use. Not all of the tasks we want to perform on a cluster are immediately visible or accessible.

One of the tasks you made need to perform is a move of all the cluster elements, Applications, CSV master and Quorum Disk Witness master to another node in your cluster you can perform necessary maintenance on the node. The task of moving the applications and CSV master is simple enough in the management interface, but for what ever reason there is no way to move the Quorum Disk Witness. Clusters (especially pre 2012 ones) are quite fickle and needy, last thing you want to do is upset them.

Rather than rely on the cluster to move the Quorum Disk Witness master for us when you power down the node we can in fact move it ourselves.

1. On the node that currently has the Quorum Disk Witness assigned to it open up an administrative command prompt
2. Input the following command:

```cmd
Cluster group “Cluster Group” /move:destinationnode
```

Just replace ```destinationnode``` with the name of the node you would like to move the Quorum Disk Witness to.

Short and sweet! Now you have that little bit of more control over your Cluster!
