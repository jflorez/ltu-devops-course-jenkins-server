# Jenkins Server for DevOps Course

This repository contains a Docker-based Jenkins server configuration designed for learning DevOps practices. The server comes pre-configured with essential plugins for continuous integration and deployment workflows.

## Prerequisites

- Docker
- Docker Compose
- Git

## Features

- Jenkins 2.491 with JDK 21
- Docker-in-Docker support for container builds
- Pre-installed plugins for:
  - Git/GitHub integration
  - Pipeline and multibranch pipeline support
  - Docker build and deployment
  - Test result analysis
  - Build tools (Ant, Gradle)
  - Authentication (LDAP, PAM)
  - Email notifications

## Quick Start

1. Clone this repository:
   ```bash
   git clone <repository-url>
   cd jenkins-server
   ```

2. Start Jenkins:
   ```bash
   docker-compose up -d
   ```

3. Access Jenkins:
   - URL: http://localhost:8088
   - Follow the initial setup wizard to create your admin account

## Configuration

### Environment Variables

- `JENKINS_PORT`: Port for Jenkins web interface (default: 8088)
- `JENKINS_HOST`: Hostname for Jenkins (default: localhost)

### Volumes

- `jenkins-data`: Persistent storage for Jenkins configuration and data
- `/var/run/docker.sock`: Docker socket for Docker-in-Docker support

## Usage in Course

This Jenkins server is configured to demonstrate:
1. Pipeline creation and management
2. Source code integration with Git
3. Automated testing and reporting
4. Docker container builds
5. Basic security and authentication setup

## Maintenance

To update Jenkins plugins:
1. Access Jenkins web interface
2. Navigate to Manage Jenkins > Plugins
3. Select plugins to update
4. Click "Download now and install after restart"

## Security Note

This setup includes:
- Matrix-based security
- LDAP integration capabilities
- PAM authentication support

For learning environments, ensure proper network isolation and access controls. 