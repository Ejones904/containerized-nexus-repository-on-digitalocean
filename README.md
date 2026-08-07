# Containerized Nexus Repository Manager on DigitalOcean

## Overview

Enterprise software development depends on reliable artifact repositories to securely store and distribute software packages, container images, and build artifacts. This project demonstrates deploying Sonatype Nexus Repository Manager as a Docker container on a DigitalOcean Ubuntu Droplet while implementing persistent storage, Linux administration, and infrastructure validation.

---

## Business Context

Organizations require centralized artifact repositories to improve software delivery, support CI/CD pipelines, and maintain version-controlled software assets. Containerizing Nexus Repository Manager simplifies deployment while Docker Volumes ensure repository data persists across container restarts and infrastructure maintenance.

This project demonstrates how containerization and cloud infrastructure can be combined to deploy enterprise repository management services.

---

## Technical Solution

Provisioned a DigitalOcean Ubuntu Droplet and configured firewall rules for secure remote administration. Installed Docker, created a persistent Docker Volume, and deployed Sonatype Nexus Repository Manager as a containerized application. Validated the deployment by confirming the Nexus web interface, inspecting Docker volumes, verifying host storage, accessing the running container, and retrieving the initial administrator password from the persistent data directory.

---

## Solution Architecture

```text
                 Administrator
                       │
                    SSH (22)
                       │
                       ▼
          DigitalOcean Ubuntu Droplet
                       │
                  Docker Engine
                       │
             Nexus Repository Manager
                       │
             Docker Volume (nexus-data)
                       │
               Persistent Host Storage
                       │
                Nexus Repository Data
```

---

## Technologies

- Docker
- Sonatype Nexus Repository Manager
- DigitalOcean
- Ubuntu Linux
- Docker Volumes
- SSH
- Linux Administration
- Cloud Networking
- Git
- GitHub

---

## Deployment Workflow

1. Provisioned a DigitalOcean Ubuntu Droplet.
2. Updated firewall rules.
3. Connected to the server using SSH.
4. Installed Docker.
5. Created a persistent Docker Volume.
6. Pulled the official Sonatype Nexus image.
7. Started Nexus as a Docker container.
8. Verified container health.
9. Confirmed port 8081 was listening.
10. Accessed Nexus through the browser.
11. Accessed the running container.
12. Verified Docker Volume configuration.
13. Inspected persistent storage.
14. Retrieved the initial administrator password.

---

## Deployment Validation

Deployment was validated by:

- Confirming the Nexus container was running.
- Verifying port 8081 was listening.
- Successfully accessing the Nexus web interface.
- Confirming Docker Volume creation.
- Inspecting Docker Volume metadata.
- Verifying persistent storage on the host.
- Retrieving the generated administrator password.

---

## Screenshots

| Screenshot | Description |
|------------|-------------|
| 07-07-30 | DigitalOcean Droplet provisioned |
| 07-07-31 | Droplet added to DigitalOcean Cloud Firewall |
| 07-07-32 | SSH connection established to the cloud server |
| 07-07-33 | Linux packages updated and Docker installed using Snap |
| 07-07-34 | Persistent `nexus-data` volume created and Nexus container deployed from the official Sonatype image |
| 07-07-35 | Port 8081 verified with `netstat` and Nexus container status confirmed with `docker ps` |
| 07-07-36 | Nexus Repository Manager web interface successfully accessed from a browser |
| 07-07-37 | Nexus container accessed with `docker exec` and runtime user verified as `nexus` |
| 07-07-38 | Persistent Docker volume verified using `docker volume ls` |
| 07-07-39 | `nexus-data` volume configuration inspected |
| 07-07-40 | Nexus data verified inside the Docker volume's host storage location |
| 07-07-41 | Initial Nexus administrator password retrieved from `/nexus-data/admin.password` |
---

## Key Achievements

- Addressed the business need for centralized artifact repository infrastructure.
- Deployed Sonatype Nexus Repository Manager as a Docker container.
- Configured persistent storage using Docker Volumes.
- Provisioned and administered Linux infrastructure in DigitalOcean.
- Validated container networking, persistent storage, and service availability.
- Retrieved and verified the initial Nexus administrator credentials.
- Produced comprehensive GitHub documentation covering deployment, validation, troubleshooting, and infrastructure administration.

---

## Lessons Learned

This project reinforced several important infrastructure concepts:

- Enterprise applications can be deployed and managed as containers.
- Persistent storage is critical for stateful infrastructure services.
- Linux administration remains fundamental in cloud environments.
- Infrastructure validation should occur at every deployment stage.
- Containerized services simplify deployment while maintaining portability.

---

## Skills Demonstrated

- Docker
- Linux Administration
- DigitalOcean
- Sonatype Nexus Repository Manager
- Docker Volumes
- SSH
- Cloud Infrastructure
- Container Administration
- Infrastructure Validation
- Technical Documentation

---

## Repository Structure

```text
containerized-nexus-repository-on-digitalocean/
│
├── README.md
├── commands.md
├── screenshots/
└── .gitignore
```

---

## Author

**Ethan Jones**

Cloud & DevOps Portfolio

- GitHub: https://github.com/Ejones904
- LinkedIn: https://www.linkedin.com/in/ethanjones-jacksonville
