---
title: "Cleaning up in Docker - Part 1"
date: "2017-07-03"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "containers"
  - "debian"
  - "docker"
  - "iot"
  - "linux"
  - "raspberry-pi"
  - "raspbian"
  - "technology"
coverImage: "docker_clean_cover.jpg"
---

So you have read [how to get Docker on your Raspberry Pi]({{< ref "blog/2017/06/setup-docker-on-raspberry-pi" >}}), had a play around and now you have containers, images and volumes running wild. You're overwhelmed, the shipis overloaded; rather than sink, how about we reset and clean up?

Pause, take a breath and following along:

## Stop all running containers

```bash
docker stop $(docker ps -a -q)
```

## Remove all containers

```bash
docker rm $(docker ps -a -q)
```

## Remove all volumes

```bash
docker volume prune -f
```

## Remove all images

```bash
docker rmi $(docker images -q)
```

Back to square one. Keep on tinkering!
