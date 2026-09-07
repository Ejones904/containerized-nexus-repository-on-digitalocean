# Cloud-Hosted Artifact Repository with Sonatype Nexus

A cloud-hosted artifact repository environment built with **Sonatype Nexus Repository Manager** on a DigitalOcean Linux server.

The project demonstrates artifact lifecycle management, repository access control, Java build-tool integration, cloud-hosted infrastructure, and troubleshooting around authenticated artifact publishing.

---

## Project Overview

The goal of this project was to build a centralized artifact repository that could receive and store Java application packages produced by multiple build tools.

The environment was designed to support:

* Sonatype Nexus Repository Manager
* Java artifact storage
* Gradle publishing
* Maven publishing
* repository users and roles
* firewall-controlled access
* Linux-based service administration

Rather than storing build outputs only on a developer workstation, the project introduced a dedicated artifact-management layer that could serve as part of a larger CI/CD workflow.

---

## Architecture

```text
                     Developer / Build Tool
                              |
                    +---------+---------+
                    |                   |
                    v                   v
                 Gradle              Maven
                    |                   |
                    +---------+---------+
                              |
                              v
                    Sonatype Nexus Repository
                              |
                              v
                   DigitalOcean Linux Server
                              |
                              v
                    Repository Artifact Store
```

Access to the Nexus server was controlled through the cloud firewall and repository-level authentication and authorization.

---

## Technology Stack

| Technology                        | Purpose                                |
| --------------------------------- | -------------------------------------- |
| Sonatype Nexus Repository Manager | Artifact repository management         |
| DigitalOcean                      | Cloud infrastructure                   |
| Ubuntu Linux                      | Nexus host operating system            |
| Java 17                           | Nexus runtime                          |
| Gradle                            | Java build and artifact publishing     |
| Maven                             | Java packaging and artifact deployment |
| SSH                               | Server administration                  |
| Git / GitHub                      | Source control                         |
| Cloud Firewall                    | Network access control                 |

---

## Engineering Decisions

### Centralized Artifact Management

A dedicated Nexus server was introduced to separate source code from compiled artifacts.

Instead of treating JAR files as local build outputs, artifacts could be published into a centralized repository where they could be stored, retrieved, and managed independently from the source repository.

This represents the same separation commonly used in CI/CD systems:

```text
Source Code
    ↓
Build
    ↓
Artifact
    ↓
Repository
    ↓
Deployment
```

---

### Support for Multiple Build Tools

Both **Gradle** and **Maven** were configured to publish artifacts into Nexus.

This validated that the repository was functioning as an independent artifact-management service rather than being tied to a single build workflow.

---

### Repository Access Control

Nexus users, roles, and repository privileges were configured to control who could publish artifacts.

This became an important part of the project because a successful application build did not automatically mean the user had permission to upload that artifact.

Repository authorization therefore had to be validated separately from the build process itself.

---

### Cloud-Level Network Controls

DigitalOcean firewall rules were configured so that only the required server access was exposed.

This separated network-level security from application-level Nexus permissions.

The resulting access model consisted of:

```text
Cloud Firewall
      ↓
Linux Host
      ↓
Nexus Authentication
      ↓
Nexus Role / Privileges
      ↓
Artifact Repository
```

---

## Implementation

A DigitalOcean Ubuntu server was provisioned and prepared as the Nexus host.

The implementation included:

* configuring the cloud firewall
* installing the required Java runtime
* installing Sonatype Nexus Repository Manager
* configuring the Linux account used to run Nexus
* starting and validating the Nexus service
* configuring Nexus users and repository roles
* integrating Gradle with the repository
* publishing and validating a Gradle artifact
* integrating Maven with the repository
* packaging and publishing a Maven artifact
* validating published artifacts through the Nexus interface

Detailed build steps and commands are preserved in [`IMPLEMENTATION.md`](IMPLEMENTATION.md).

---

## Artifact Publishing Workflow

The artifact lifecycle implemented in this project follows this pattern:

```text
Application Source
       |
       v
 Gradle / Maven
       |
       v
 Application Build
       |
       v
    JAR Artifact
       |
       v
Authenticated Publish
       |
       v
 Sonatype Nexus
       |
       v
 Central Artifact Store
```

This provides a clean separation between application development, artifact generation, and artifact storage.

---

## Troubleshooting

### Gradle Artifact Publish Failure

One of the most significant issues occurred when the Gradle build completed successfully but the artifact could not be published to Nexus.

The failure initially appeared to be part of the build workflow, but investigation showed that application compilation and repository publishing were separate stages.

The root cause was insufficient Nexus permissions for the publishing user.

The repository role and privileges were reviewed and updated to allow the required artifact operations.

After correcting the authorization configuration, the same publishing workflow completed successfully.

![Gradle Publish Failure](screenshots/22-troubleshoot-gradle-publish.png)

![Gradle Publish Success](screenshots/23-publish-success.png)

This reinforced an important troubleshooting distinction:

```text
Build Failure
     ≠
Repository Authentication Failure
     ≠
Repository Authorization Failure
```

Each layer has to be investigated independently.

---

## Validation

Validation was performed at several layers.

### Repository Access

Confirmed that the Nexus web interface was reachable through the configured cloud firewall.

### User and Role Configuration

Verified that repository users were assigned the required roles and privileges.

<!-- Use whichever screenshot most clearly shows the final permissions state. -->

![Nexus Role Configuration](screenshots/17--create-user-role.png)

### Gradle Publishing

Confirmed that the Gradle application could build and publish its artifact after the permissions issue was resolved.

![Gradle Artifact in Nexus](screenshots/24-UI-validation.png)

### Maven Publishing

The Maven application was packaged into a JAR and deployed to Nexus using Maven.

![Maven Deploy Success](screenshots/28--mvn-deploy-success.png)

The resulting artifact was then verified inside the Nexus repository interface.

![Maven Artifact Validation](screenshots/29--ui-validation.png)

Together, these tests validated the complete artifact path:

```text
Source Code
    ↓
Build Tool
    ↓
JAR Generation
    ↓
Authenticated Repository Upload
    ↓
Nexus Artifact Storage
```

---

## Security Considerations

The project includes several security controls appropriate for a hands-on engineering environment:

* cloud firewall rules limiting server access
* authenticated Nexus users
* role-based repository permissions
* separation between administrative and publishing privileges
* Linux ownership and service configuration

For a production implementation, additional controls would be required.

These would include:

* TLS/HTTPS for Nexus access
* stronger secret-management practices
* least-privilege service accounts
* restricting administrative access to trusted networks
* repository backup and recovery procedures
* audit logging and access review
* external identity-provider integration where appropriate
* regular operating system and Nexus patching

---

## Operational Considerations

The Nexus service required manual administration during the project, including restarting the application after periods when the environment was not in use.

A more mature deployment would run Nexus as a managed Linux service so that service startup, restart behavior, and failure handling could be controlled through the operating system.

For example, a future implementation could use `systemd` to support:

* automatic startup after reboot
* standardized service status checks
* restart policies
* centralized service logs

---

## What This Project Demonstrates

This project demonstrates practical experience with:

* artifact repository administration
* Sonatype Nexus Repository Manager
* cloud-hosted Linux infrastructure
* Java artifact lifecycle management
* Gradle publishing
* Maven publishing
* role-based access control
* repository permissions
* firewall configuration
* SSH administration
* troubleshooting authorization failures
* validating artifacts across multiple build systems

---

## Future Enhancements

Potential improvements include:

* running Nexus as a `systemd` service
* enabling TLS/HTTPS
* integrating Jenkins with Nexus
* automating Nexus installation and configuration
* implementing repository backup procedures
* defining artifact retention policies
* adding infrastructure monitoring
* integrating centralized logging
* externalizing credentials
* provisioning the environment with Terraform

---

## Repository Documentation

* [`README.md`](README.md) — engineering overview, design decisions, and validation
* [`IMPLEMENTATION.md`](IMPLEMENTATION.md) — detailed implementation history

---

## Engineering Outcome

This project established a centralized artifact-management layer capable of receiving Java packages from multiple build systems.

More importantly, the implementation demonstrated that successful software delivery depends on more than compiling an application. Network access, authentication, authorization, artifact storage, and validation all operate as separate layers that must work together for a complete delivery workflow.

