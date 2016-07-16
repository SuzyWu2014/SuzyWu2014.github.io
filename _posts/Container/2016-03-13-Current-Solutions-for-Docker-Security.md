---
layout: post
title: "Current Solutions for Docker Security"
description: ""
category: Docker
tags:  [Docker]
---
{% include JB/setup %}

This article summarizes the current security solutions for  Docker containers. I will review the solutions that are brought by the Docker implementation, then discuss how to enhance it when running Docker in production.

## Possible Security Issues in a container-based environment
Before we jump into the security solutions, let's exploring some of the issues surrounding the security of container-based systems. Generally speaking, there are three types of attack models, which are caused by the vulnerabilities of the container-based systems.

### Types of Attacks:

- Container compromise: result in illegitimate data access and affect control flow of instructions
- DoS(Deny of Services): disturb normal operation of the host or other container
- Privilege escalation: obtain a provilege which is not originally granted to the container

### Disclosed Vulnerabilities:

- Namespacing Issues
Docker containers utilize Kernel namespaces to provide a certain level of isolation. However, not all resources are namespaced:
	* UID: Causing "root" user vulnerability
	* Kernel keyring: containers running with a user of the same UID will have access to the same keys if they are handled by kernel keyring
	* Kernel & its modules: Loaded modules become available across all containers and the hot
	* Devices: includes disk drives, sound-cards,GPU, etc.
	* System time: The SYSTEM_TIME capability is disabled by default, but if it's enabled, we will need to worry about it.
-  Kernel Exploit
Container-based applications share the same host kernel, namely, flaws in  the host kernel might allow malicious containers to escape and gain access over the over whole system.
- DoS Attacks
Since all containers share kernel resources, if one container or one user monopolize access to certain resource, it will starve out other containers on the host.
- Container Breakout
Because users are not namespaced, any process that break out if the container will have the same privileges on the host as it did in the container. For example, if you were root in the container, you will be root on the host. It's a typical privilege escalation attack , unlikely happen, but possible.
- Poisoned Images
It's possible for attackers ro modify/embed malicious program into the image and trick users to download such corrupt images
- Compromising secrets
It's likely to require a secret to access a database or sevices. An attacker who can get access to this secret will also have access to the service.This problem becomes more acute in a microservice architecture in which containers are constantly stopping and starting.

## Current Solutions:
Now Let's take a look at what security solutions that come with Docker implementation and what strategies or techniques that are used in production.

### Least Privileges
One of the most important principle to achieve container security is Least Privileges: each process and container should run with the minimum set of access rights and resources it needs to perform its function. This includes the actions to reduce the capabilities of containers:

+  Do not run processes in a container as root to avoid root access from attackers.
+  Run filesystems as read-only so that attackers can not overwrite data or save malicious scripts to file.
+  Cut down the kernel Calls that a container can make to reduce the potential attack surface.
+  Limit the resources that a container can use

This Least Privileges approach reduced the possibility that the attacker can access or exploit data or resources via a compromised container.

### Internal Security Solutions
Containers can leverage on Linux Namespace and Control group to provide a certain level of isolation and resource limitation.

#### Namespace
Docker provides process, filesystem, device, IPC and net- work isolations by using the related namespace.

- Process Isolation: Docker utilizes PID namespace to separate container processes from the host as well as other containers, so that processes in a container can’t observe or do anything to the other processes running in the host or in other containers.
- Filesystem Isolation: Use mount namespace to ensure that for each mount space, a container only have impact inside the container.
- Device Isolation: The container cannot access to any devices unless it’s privileged.
- IPC Isolation: Utilize IPC namespace to prevent the processes in a container from interfering with those in other containers.
- Network Isolation: Use network namespace so that each container has its own IP address, IP routing tables, network device, etc.

#### Control Group
Docker employs Cgroup to control the amount of resources, such as CPU, memory, and disk I/O, that a container can use. Under this control, each container are guaranteed a fair share of the resources but preventing from consuming all resources.

### Linux Kernel Security Systems
Kernel security system, such as Linux capabilities are existed to harden the security of a Linux host system. We can also use them to secure the host from containers.

By default, containers disable a large amount of Linux capabilities from its containers in order to prevent an attacker to damage the host system when a container is compromised. And it also allows configuration of capabilities that a container can use.

### inux Security Module(LSM)
Two most popular LSM will be AppArmor and SELinux.

#### SELinux
SElinux is a labeling system, that implements Mandatory access control using labels. Every object, such as process, file/directory, network ports, devices, etc, has a label, and we write riles to control the access of an object.

#### AppArmor
AppArmor is a security enhancement model to Linux based on Mandatory Access Control like SELinux. It permits the administrator to load a security profile into each program, which limits the capabilities of the program.

### Other Approach

#### Seccomp
The Linux seccomp(or secure computing mode) facility can be used to restrict the system calls that can be made by a process. namely, containers can be locked down to a specified set of system calls.

## In Production
In production, we  leverage on the security solutions we have mentioned above and apply proper operations in order to provide a more secure and efficient system. There are major three secure tips in production.

### Segregate Containers by Host
The main reason to place each user on a separate Docker Host is to minimize the loss when constainer breakout happens. If multiple users are sharing one host, if a user monipolizes all the memory on the host, it will starve out other users. Even worse, if constainer breakouts happen, a user could possibly gain access to another users' containers or data through the compromised container.

Therefore, although this approach is less efficient than sharing hosts between users and will result in a highter number of VMs and/or machines than reusing hosts, it's important for security.

Another similar solution would be separate containers with sensitive  information from less-sensitive ones for the similar reason.

### Applying Updates
Just like what is recommended for Windows system, it's recommended to apply updates regularly. This includes updating base images and dependent images to fix the vulnerabilities in common utilities and framework. At times, we need to update Docker daemon to gain access to new feature, security patches or bug fixes. Removing unsupported drivers is also important, because those could be a security risk since it won't be receiving the same attension and updates as other parts of Docker.

### Image Provenance
To safely use images, you need to have guarantees about their provenance:

- where they came from
- who created them
- essure you are getting the exactly the image you want

There are three solutions  for image provenace: secure hash, secure signing and verification infrastructure and use Dockerfile properly.

- Secure Hash:  Secure Hash is like a fingerprint for data. It's a small string that is unique to given data. If you have a secure hash for some data and the data itself, you can recalculate the hash for the data then compare.  In docker, it's called docker digest, a SHA-256 hash of a filesystem layer or manifest(a metadata file describing the parts of a image, constaining a list of constituent layer identified by digest)
- Secure Signing and Verification Infrastructure:  Data could be changed if it transits over untrustworthy channels(e.g. HTTP), so we need to ensure we are publishing and accessing content in a trustworthy and secure manner. Notaty project is an on-going secure signing and verification infrastructure project in docker, which compares a checksum for a downloaded file with the checksum in Notary's trusted collection for the file source (e.g. docker.com ). For more details, please check https://github.com/docker/notary
- Dockerfile:  Not as we expected, dockerfile is likely to produce different images over time, so as time goes, it's hard to be sure what is in your images. To use docker properly, you would:
	* Always specify a tag in FROM instruction, and use digest to pull the exactly same image each time
	* Provide version numbers when installing software from package managers. However, since package dependencies can change over time, sometime we need to use tools (e.g. aptly) to take a snapshot of the repository
	* Verify any software or data download from the internet by checksum or cryptographic.

END: This article is an glance of the current security solutions for docker containers, if you are interested, please refer to the reference articles for more details.

## References:
[1] Analysis of Docker Security
[2] Docker Security - Using Containers Safely in Production
[3] Docker Doc - Docker Security
