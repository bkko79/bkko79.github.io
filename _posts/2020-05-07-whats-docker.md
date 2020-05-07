---
title: "Docker A to Z in 10..ish minutes"
date: 2020-05-18 00:18:30 +0900
categories: Knowledges
comments: true
---
## So what is Docker

What is Docker in one line? It is an Application-level software that allows Container Process to virtualize environment above host OS. If you're not keen on any of words in description, I will explain each of them.

## Container; _chroot on steroids_

So, what is _Container_? Before Container, _Virtualization_ software installed OS above _virtualized hardware_ to create new environment. That's a hell of cost in both time and space to emulate thousands of it. But Container took the other path. It is thinner layer of virtualization, _admitting that OS already exists_. It uses the same HOST OS' Kernel ABI(Binary Interface), so doesn't need to install whole new guest OS. (That is the only thing you have to remember.)

In 1995, [Solaris Container][solaris] brought a word _Container_ over the computation industry for the first time. Briefly, it enlightened the world with its broad concept of thinner virtualization, isolating process with chroot and _zones_([LXC][LXC] took it and evolved into _namespaces_), which became a fundamental concept for Docker.

Chroot only allowed user to isolate certain directory and to look like a root directory, but Solaris Container took whole another level to create isolated environment. (that's why it was called _chroot on steroid_.) LXC took the concept, and developed it as a Linux application.

![LXC](/assets/img/containers.png)

Docker was built in Linux environment with the container runtime: LXC, but now the Container runtime is replaced with runC. But it is still the same thing. It looks like a virtual environment, but it is just a process that imitates it.

## Not a machine, it's just one of _proc_

If you're familiar with VMware, Vagrant and others, this is the point where you get confused. But it is _not_ a different machine at all. Everything is running on Host OS.

Proof 1. We don't need and OS installation iso file, which weighs hundreds of megabytes. Docker hub provides official _images_ which imitates the environment.

Proof 2. It is much faster than launching a VM. (Milliseconds level of performance)

Proof 3. Guest OS' File system is separated into designated directory, and you can access it from HOST OS' file system easily. (Which is controversially critical.)

Here's a [great article][jvns] about Docker's _proc_ characteristics.

## Pros and Cons

### Pros

1. Faster than VM.
2. You don't need Guest OS. Less memory consumption and better disk management.

### Cons

1. Vague layer of isolation & limitation.
2. Union File system performance is still limited.
3. Each Kernel's limitation becomes Docker's limitation (Linux cannot execute Windows 10 based image).
4. Higher level of abstraction in Network layer.

It is not only Docker's case but inherent limit for thinner layer of isolation.

## So, what is Layer

![Demystifying](/assets/img/Demystifying-containers_image1.png)

Think about Version Control system. Everything is _layered_. Then, what is layer in Docker Container? It is a _container layer wrapping image layers_ that allows you to launch it as one process in OS.

Each image become a layer which contains _diff_ results that built over original image. That customized image with thousands of layers are wrapped by Container layer.

```shell
FROM ubuntu:18.04
COPY . /app
RUN make /app
CMD python /app/app.py
```

For example, this dockerfile overrides original ubuntu layer and make a new image that contains its own application. If you don't get it, you should try to make a [_dockerfile_][dockerfile] to understand what I mean.

## How Docker communicates

After we deploy a container with any application, we need to attach network to serve it. As I explained earlier, everything is running in Host OS, logically isolated process, therefore any lower level of Network configuration that were available in Linux might not be an option here.

As I mentioned earlier about higher level of abstraction in Network, Docker has few Network presets that user can choose while they launch container:

1. bridge(default): Containers linked to bridge-network.
2. Host: It is bound directly to host port.
3. Overlay: Presets for Docker Swarm Orchestration with ingress & gwbridge rule.
4. Macvlan: Each container gets virtual MAC Address for specific macvlan networks.

## Too many containers to launch? _docker-compose_

Almost done. If you're aware of [microservice architecture][microservice], you would notice that an application is consist of more than one container that communicates each other. Launching all these stacked containers is very time-consuming job, therefore docker-compose file written in yaml allows administrator to deploy multi container application.

## Closing

Not just a environment, I've been using docker in different ways like: Using centOS image to build a rpm file from ubuntu. I just mount rpm spec files over centOS container, and send specific CMD lines to run _spectool, rpm-sign, createrepo_ and other tools to build a package file.

In this way I can save my time installing whole centOS VM. Docker just became a wonderful command line tool.

[solaris]: https://en.wikipedia.org/wiki/Solaris_Containers
[LXC]: https://en.wikipedia.org/wiki/LXC
[chroot]: https://en.wikipedia.org/wiki/Chroot
[jvns]: https://jvns.ca/blog/2020/04/29/why-strace-doesnt-work-in-docker/
[dockerfile]: https://docs.docker.com/develop/develop-images/dockerfile_best-practices/
[microservice]: https://microservices.io/
