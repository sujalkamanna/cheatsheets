# Podman

# 🦭 Podman Complete Cheatsheet

![Podman Logo](https://podman.io/images/logo/podman-logo.svg)

---

# Table of Contents

1. [What is Podman?](#1-what-is-podman)
2. [Core Concepts](#2-core-concepts)
3. [Podman Architecture](#3-podman-architecture)
4. [Installation](#4-installation)
5. [Getting Started](#5-getting-started)
6. [Container Lifecycle Commands](#6-container-lifecycle-commands)
7. [Image Management](#7-image-management)
8. [Container Networking](#8-container-networking)
9. [Volumes and Storage](#9-volumes-and-storage)
10. [Pods in Podman](#10-pods-in-podman)
11. [Rootless Containers](#11-rootless-containers)
12. [Security Features](#12-security-features)
13. [Registries and Authentication](#13-registries-and-authentication)
14. [Docker Compatibility](#14-docker-compatibility)
15. [Podman Compose](#15-podman-compose)
16. [Systemd Integration](#16-systemd-integration)
17. [Kubernetes Integration](#17-kubernetes-integration)
18. [Troubleshooting](#18-troubleshooting)
19. [Best Practices](#19-best-practices)
20. [Additional Resources](#20-additional-resources)

---

# 1. What is Podman?

Podman (Pod Manager) is an open-source container engine used to develop, manage, and run OCI (Open Container Initiative) containers.

It is designed as a daemonless alternative to Docker and is widely adopted in Linux, DevOps, Platform Engineering, Kubernetes, and Enterprise environments.

Unlike Docker, Podman does not require a central daemon process.

```text
Docker
   |
Docker Daemon
   |
Containers

-----------------------

Podman
   |
Containers
(No Daemon Required)
```

### Key Benefits

* Daemonless architecture
* Rootless containers
* OCI compliant
* Docker-compatible CLI
* Better security model
* Native Pod support
* Kubernetes integration
* Systemd integration

---

# 2. Core Concepts

| Concept       | Description                                  |
| ------------- | -------------------------------------------- |
| Container     | Running instance of an image                 |
| Image         | Read-only template used to create containers |
| Registry      | Repository storing container images          |
| Pod           | Group of containers sharing resources        |
| Volume        | Persistent storage                           |
| Namespace     | Linux isolation mechanism                    |
| Cgroups       | Resource limitation mechanism                |
| OCI           | Open Container Initiative standard           |
| Rootless      | Running containers without root privileges   |
| Containerfile | Podman equivalent of Dockerfile              |

---

# 3. Podman Architecture

## High-Level Architecture

```text
                 User
                   |
                   v
              Podman CLI
                   |
        -----------------------
        |                     |
        v                     v
  Container Runtime      Image Manager
      (runc/crun)         (containers/image)
        |                     |
        -----------------------
                   |
                   v
              Linux Kernel
```

---

## Docker vs Podman

```text
Docker

CLI
 |
Docker Daemon
 |
Container Runtime
 |
Containers

--------------------------------

Podman

CLI
 |
Container Runtime
 |
Containers

(No Daemon)
```

### Advantages Over Docker

* No daemon attack surface
* Better security
* Easier debugging
* Rootless execution
* Native pods

---

# 4. Installation

## RHEL / Rocky Linux / AlmaLinux

```bash
sudo dnf install podman -y
```

Verify:

```bash
podman --version
```

---

## CentOS Stream

```bash
sudo dnf install podman -y
```

---

## Fedora

```bash
sudo dnf install podman -y
```

---

## Ubuntu

```bash
sudo apt update

sudo apt install podman -y
```

---

## Debian

```bash
sudo apt update

sudo apt install podman -y
```

---

## macOS

```bash
brew install podman
```

Create machine:

```bash
podman machine init

podman machine start
```

---

## Windows

```powershell
winget install RedHat.Podman
```

Create machine:

```powershell
podman machine init

podman machine start
```

---

# 5. Getting Started

Check Podman info:

```bash
podman info
```

Check version:

```bash
podman version
```

List images:

```bash
podman images
```

List running containers:

```bash
podman ps
```

List all containers:

```bash
podman ps -a
```

Display system information:

```bash
podman system info
```

Display disk usage:

```bash
podman system df
```

---

# 6. Container Lifecycle Commands

## Pull Image

```bash
podman pull nginx
```

Specific tag:

```bash
podman pull nginx:latest
```

---

## Create Container

```bash
podman create nginx
```

Named container:

```bash
podman create --name web nginx
```

---

## Run Container

```bash
podman run nginx
```

Detached mode:

```bash
podman run -d nginx
```

Interactive shell:

```bash
podman run -it ubuntu bash
```

Assign name:

```bash
podman run -d --name web nginx
```

Port mapping:

```bash
podman run -d \
-p 8080:80 \
nginx
```

---

## Start Container

```bash
podman start web
```

---

## Stop Container

```bash
podman stop web
```

Force stop:

```bash
podman kill web
```

---

## Restart Container

```bash
podman restart web
```

---

## Pause Container

```bash
podman pause web
```

Resume:

```bash
podman unpause web
```

---

## Remove Container

```bash
podman rm web
```

Force remove:

```bash
podman rm -f web
```

---

## Execute Command

```bash
podman exec -it web bash
```

Run command:

```bash
podman exec web ls -l
```

---

## Container Logs

```bash
podman logs web
```

Follow logs:

```bash
podman logs -f web
```

Last 100 lines:

```bash
podman logs --tail 100 web
```

---

## Container Inspection

```bash
podman inspect web
```

Specific value:

```bash
podman inspect web | jq .
```

---

## Container Statistics

```bash
podman stats
```

Single container:

```bash
podman stats web
```

---

## Top Processes

```bash
podman top web
```

---

## Rename Container

```bash
podman rename oldname newname
```

---

## Export Container

```bash
podman export web > web.tar
```

---

## Import Container

```bash
podman import web.tar
```

---

## Container Checkpoint

```bash
podman container checkpoint web
```

Restore:

```bash
podman container restore web
```

---

# 7. Image Management

## Pull Image

```bash
podman pull nginx
```

## Search Registry

```bash
podman search nginx
```

## List Images

```bash
podman images
```

## Inspect Image

```bash
podman image inspect nginx
```

## Remove Image

```bash
podman rmi nginx
```

## Force Remove

```bash
podman rmi -f nginx
```

## Save Image

```bash
podman save nginx -o nginx.tar
```

## Load Image

```bash
podman load -i nginx.tar
```

## Tag Image

```bash
podman tag nginx myrepo/nginx:v1
```

## Image History

```bash
podman history nginx
```

---

## 8. Container Networking

Podman provides multiple networking modes and supports rootless networking using Netavark and Aardvark DNS.

### View Networks

```bash
podman network ls
````

### Inspect Network

```bash
podman network inspect podman
```

### Create Network

```bash
podman network create mynet
```

### Run Container on Network

```bash
podman run -d \
--network mynet \
--name web nginx
```

### Connect Existing Container

```bash
podman network connect mynet web
```

### Disconnect Container

```bash
podman network disconnect mynet web
```

### Remove Network

```bash
podman network rm mynet
```

### Port Mapping

```bash
podman run -d \
-p 8080:80 \
nginx
```

```text
Host Port 8080
      |
      v
Container Port 80
```

---

## 9. Volumes and Storage

Volumes provide persistent storage beyond container lifecycle.

### Create Volume

```bash
podman volume create mydata
```

### List Volumes

```bash
podman volume ls
```

### Inspect Volume

```bash
podman volume inspect mydata
```

### Mount Volume

```bash
podman run -d \
-v mydata:/data \
nginx
```

### Bind Mount

```bash
podman run -d \
-v /host/path:/container/path \
nginx
```

### Read Only Mount

```bash
podman run -d \
-v /host/data:/data:ro \
nginx
```

### Remove Volume

```bash
podman volume rm mydata
```

### Prune Unused Volumes

```bash
podman volume prune
```

---

## 10. Pods

A Pod is a group of containers sharing:

* Network namespace
* IPC namespace
* Ports
* Storage

Similar to Kubernetes Pods.

### Create Pod

```bash
podman pod create \
--name mypod
```

### Create Pod with Port Mapping

```bash
podman pod create \
--name mypod \
-p 8080:80
```

### List Pods

```bash
podman pod ls
```

### Inspect Pod

```bash
podman pod inspect mypod
```

### Run Container Inside Pod

```bash
podman run -dt \
--pod mypod \
nginx
```

### Stop Pod

```bash
podman pod stop mypod
```

### Start Pod

```bash
podman pod start mypod
```

### Remove Pod

```bash
podman pod rm mypod
```

### Force Remove

```bash
podman pod rm -f mypod
```

---

## 11. Rootless Containers

One of Podman's biggest advantages.

Containers run without root privileges.

```text
Traditional Container

Root User
    |
Container

--------------------

Podman Rootless

Normal User
      |
Container
```

### Verify Rootless Mode

```bash
podman info
```

Look for:

```text
rootless: true
```

### Run Rootless Container

```bash
podman run -d nginx
```

No sudo required.

### Benefits

* Reduced attack surface
* Better isolation
* Improved security
* Multi-user support

---

## 12. Security Features

### User Namespace Isolation

```bash
podman run \
--userns=keep-id \
ubuntu
```

### Read Only Container

```bash
podman run \
--read-only \
nginx
```

### Drop Linux Capabilities

```bash
podman run \
--cap-drop ALL \
nginx
```

### Add Specific Capability

```bash
podman run \
--cap-add NET_ADMIN \
ubuntu
```

### SELinux Support

```bash
podman run \
-v /data:/data:Z \
nginx
```

### Resource Limits

CPU Limit:

```bash
podman run \
--cpus=2 \
nginx
```

Memory Limit:

```bash
podman run \
--memory=512m \
nginx
```

---

## 13. Registries and Authentication

### Login

```bash
podman login docker.io
```

### Login to Private Registry

```bash
podman login registry.example.com
```

### Logout

```bash
podman logout docker.io
```

### Show Login File

```bash
cat ~/.config/containers/auth.json
```

---

## 14. Working with Registries

### Push Image

```bash
podman push \
myimage:v1
```

### Pull Image

```bash
podman pull nginx
```

### Tag Before Push

```bash
podman tag nginx \
docker.io/user/nginx:v1
```

### Push Tagged Image

```bash
podman push \
docker.io/user/nginx:v1
```

### Common Registries

```text
Docker Hub
Quay.io
GitHub Container Registry
Amazon ECR
Google Artifact Registry
Azure Container Registry
Harbor
```

---

## 15. Build Images

### Build from Containerfile

```bash
podman build -t myapp .
```

### Custom File

```bash
podman build \
-f Containerfile \
-t myapp .
```

### Build Without Cache

```bash
podman build \
--no-cache \
-t myapp .
```

### Example Containerfile

```Dockerfile
FROM nginx

COPY . /usr/share/nginx/html

EXPOSE 80

CMD ["nginx","-g","daemon off;"]
```

---

## 16. Docker Compatibility

Podman is designed as a drop-in replacement for many Docker workflows.

### Docker Equivalent Commands

| Docker        | Podman        |
| ------------- | ------------- |
| docker run    | podman run    |
| docker ps     | podman ps     |
| docker images | podman images |
| docker build  | podman build  |
| docker pull   | podman pull   |
| docker push   | podman push   |
| docker exec   | podman exec   |
| docker logs   | podman logs   |
| docker stop   | podman stop   |
| docker rm     | podman rm     |

### Docker Alias

```bash
alias docker=podman
```

Verify:

```bash
docker ps
```

### Generate Docker-Compatible CLI

```bash
podman-docker
```

Install:

```bash
sudo dnf install podman-docker
```

or

```bash
sudo apt install podman-docker
```

---

## 17. Podman Compose

Podman supports Docker Compose style deployments.

### Install

```bash
pip install podman-compose
```

Verify:

```bash
podman-compose version
```

### Start Application Stack

```bash
podman-compose up
```

Detached Mode

```bash
podman-compose up -d
```

### Stop Stack

```bash
podman-compose down
```

### View Logs

```bash
podman-compose logs
```

### Example Compose File

```yaml
version: "3"

services:

  web:
    image: nginx
    ports:
      - "8080:80"

  db:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: password
```

---

## 18. Systemd Integration

One of Podman's strongest features is native Systemd integration.

### Generate Systemd Service

```bash
podman generate systemd \
--name web
```

Save Service

```bash
podman generate systemd \
--name web \
--files
```

Generated:

```text
container-web.service
```

### Enable Service

```bash
systemctl --user enable container-web.service
```

Start:

```bash
systemctl --user start container-web.service
```

Status:

```bash
systemctl --user status container-web.service
```

### Auto Start After Reboot

```bash
loginctl enable-linger $USER
```

---

## 19. Kubernetes Integration

Podman can generate Kubernetes manifests.

### Generate YAML

```bash
podman generate kube mypod
```

Output:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
```

Save to File

```bash
podman generate kube mypod > pod.yaml
```

### Deploy Generated Manifest

```bash
kubectl apply -f pod.yaml
```

### Create Pod from Kubernetes YAML

```bash
podman play kube pod.yaml
```

Remove Resources

```bash
podman kube down pod.yaml
```

### Why Useful?

```text
Local Development
        |
        v
Podman Pod
        |
Generate YAML
        |
        v
Kubernetes Cluster
```

---

## 20. Troubleshooting

### Container Won't Start

Check logs:

```bash
podman logs container_name
```

Inspect container:

```bash
podman inspect container_name
```

---

### Port Not Accessible

Verify mapping:

```bash
podman port container_name
```

Check listening ports:

```bash
ss -tulpn
```

---

### Storage Issues

Check disk usage:

```bash
podman system df
```

Cleanup:

```bash
podman system prune
```

---

### Image Pull Failure

Verify registry access:

```bash
podman login registry.example.com
```

Test connectivity:

```bash
curl registry.example.com
```

---

### Rootless Networking Issues

Verify user namespaces:

```bash
podman unshare cat /proc/self/uid_map
```

---

## 21. Best Practices

### Use Rootless Containers

```bash
podman run nginx
```

Avoid:

```bash
sudo podman run nginx
```

unless absolutely required.

---

### Use Specific Tags

Good:

```bash
podman pull nginx:1.28
```

Avoid:

```bash
podman pull nginx:latest
```

---

### Apply Resource Limits

```bash
podman run \
--memory=512m \
--cpus=2 \
nginx
```

---

### Use Read-Only Containers

```bash
podman run \
--read-only \
nginx
```

---

### Remove Unused Resources

```bash
podman system prune
```

---

### Store Persistent Data in Volumes

```bash
podman volume create appdata
```

---

### Scan Images Before Production

Use trusted registries and signed images whenever possible.

---

## 22. Common Commands Quick Reference

### Containers

```bash
podman run nginx
podman ps
podman ps -a
podman stop container
podman start container
podman restart container
podman rm container
podman exec -it container bash
podman logs container
```

### Images

```bash
podman pull nginx
podman images
podman build -t app .
podman tag app repo/app:v1
podman push repo/app:v1
podman rmi image
```

### Networks

```bash
podman network ls
podman network create mynet
podman network inspect mynet
podman network rm mynet
```

### Volumes

```bash
podman volume create mydata
podman volume ls
podman volume inspect mydata
podman volume rm mydata
```

### Pods

```bash
podman pod create
podman pod ls
podman pod inspect pod
podman pod stop pod
podman pod rm pod
```

### System

```bash
podman info
podman version
podman system df
podman system prune
```

---

# 📎 Official Documentation

* https://podman.io/
* https://docs.podman.io/
* https://github.com/containers/podman
* https://github.com/containers/buildah
* https://github.com/containers/skopeo
* https://github.com/containers/common

---

# 📎 Disclaimer & Attribution

This document is an unofficial educational cheatsheet created for learning, revision, interview preparation, DevOps engineering, cloud engineering, platform engineering, site reliability engineering, Linux administration, and container platform administration.

Primary references include:

* Podman Documentation
* Podman GitHub Repository
* OCI Specifications
* Buildah Documentation
* Skopeo Documentation

Podman is an open-source container engine maintained by the Containers Organization and Red Hat.

All trademarks, product names, and service names belong to their respective owners.

For authoritative information, always refer to official documentation.

---

# 🎉 Conclusion

Podman provides:

```text
Daemonless Container Engine
+
Rootless Containers
+
OCI Compliance
+
Pod Support
+
Kubernetes Integration
+
Systemd Integration
+
Enhanced Security
+
Docker Compatibility
+
Enterprise Reliability
````

and serves as a modern container platform for DevOps, cloud-native applications, Kubernetes development, platform engineering, and enterprise Linux environments.

---

```
**Last Updated:** 2026
**Platform:** Podman
**Version:** Podman 5.x+
**License:** Apache License 2.0
```
For latest information, visit [https://docs.podman.io/](https://docs.podman.io/)