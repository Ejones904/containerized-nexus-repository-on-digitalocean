# Containerized Nexus Repository on DigitalOcean

This document contains the commands used to deploy, validate, and administer Sonatype Nexus Repository Manager as a Docker container on a DigitalOcean Ubuntu Droplet.

---

## Connect to the Server

Connect to the DigitalOcean Droplet.

```bash
ssh root@192.241.131.228
```

---

## Update the Server

Update the package index.

```bash
apt update
```

---

## Install Docker

Install Docker using Snap.

```bash
snap install docker
```

Verify Docker.

```bash
docker --version
```

---

## Create Persistent Storage

Create a Docker volume for Nexus.

```bash
docker volume create --name nexus-data
```

---

## Deploy Nexus Repository Manager

Download and start the Nexus container.

```bash
docker run -d \
-p 8081:8081 \
--name nexus \
-v nexus-data:/nexus-data \
sonatype/nexus3
```

---

## Validate the Deployment

Verify the Nexus container.

```bash
docker ps
```

Verify the listening port.

```bash
netstat -lnpt
```

Open Nexus.

```
http://192.241.131.228:8081
```

---

## Container Administration

Access the running container.

```bash
docker exec -it 993ec62ff03f /bin/sh
```

Verify the container user.

```bash
whoami
```

Exit the container.

```bash
exit
```

---

## Docker Volume Validation

List Docker volumes.

```bash
docker volume ls
```

Inspect the Nexus volume.

```bash
docker inspect nexus-data
```

Locate the volume on the Docker host.

```bash
ls /var/snap/docker/common/var-lib-docker/volumes/nexus-data/_data
```

---

## Retrieve Initial Admin Password

Access the running container.

```bash
docker exec -it 993ec62ff03f /bin/sh
```

Navigate to the Nexus data directory.

```bash
cd /nexus-data
```

Display the initial administrator password.

```bash
cat admin.password
```

Exit the container.

```bash
exit
```

---

## Useful Docker Commands

Running containers.

```bash
docker ps
```

Docker images.

```bash
docker images
```

Docker volumes.

```bash
docker volume ls
```

Inspect a Docker volume.

```bash
docker inspect nexus-data
```

Stop Nexus.

```bash
docker stop nexus
```

Start Nexus.

```bash
docker start nexus
```

Restart Nexus.

```bash
docker restart nexus
```

---

## Skills Demonstrated

- Docker
- Linux Administration
- DigitalOcean
- Docker Volumes
- Container Administration
- SSH
- Infrastructure Validation
- Persistent Storage
- Technical Documentation
