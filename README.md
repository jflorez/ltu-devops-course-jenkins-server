# Jenkins Server for DevOps Course

This repository contains a containerized Jenkins setup designed for learning DevOps practices. It features a Jenkins server and a custom Node.js agent configured using Infrastructure as Code principles.

## Features

### Jenkins Configuration as Code (JCasC)
- Automated server configuration using `jenkins.yaml`
- Zero-executor security policy on master node
- Docker-based agent configuration
- Git integration settings

### Docker Integration
- Docker-in-Docker capability for building and running containers in pipelines
- Custom Node.js 22 agent image
- Dynamic agent provisioning using Docker
- Secure Docker socket mounting

### Agent Configuration
- Automated agent provisioning using Docker
- Node.js 22 environment for running Node.js applications
- Docker capabilities enabled
- Maximum of 10 concurrent instances
- Privileged mode for full Docker functionality

## Prerequisites
- Docker
- Docker Compose

## Setup Instructions

1. Start Jenkins (this will build the agent image and start the server):
```bash
docker compose up -d
```

2. Access Jenkins at: http://localhost:8088

## Environment Variables
- `JENKINS_PORT`: Server port (default: 8088)
- `JENKINS_HOST`: Server hostname (default: localhost)
- `TZ`: Timezone (default: Australia/Melbourne)

## Understanding the Components

### Jenkins Server (`build/jenkins-server/`)
- `Dockerfile`: Base Jenkins server with Docker support
- `jenkins.yaml`: JCasC configuration file
- `plugins.txt`: Pre-installed Jenkins plugins

### Node.js Agent (`build/jenkins-agent-node22/`)
- `Dockerfile`: Custom agent with Node.js 22
- Designed specifically for building and testing Node.js applications

## Security Notes
- Master node runs with zero executors for security
- Docker socket is mounted for Docker-in-Docker operations
- Agent runs in privileged mode for full Docker functionality
- Git host key verification is disabled for learning purposes (not recommended for production)

## Learning Objectives
This setup demonstrates several key DevOps concepts that can be applied across various CI/CD tools:
1. Infrastructure as Code (IaC) for automated configuration management
2. Container orchestration and management
3. Effective use of build agents and executors
4. Security best practices in continuous integration and delivery
5. Environment configuration and management for consistency and reliability 

## Usage in Pipelines

Example pipeline using the Node.js agent:
```groovy
pipeline {
    agent {
        label 'node'
    }
    stages {
        stage('Test Environment') {
            steps {
                sh 'node --version'
                sh 'npm --version'
                sh 'docker --version'
            }
        }
    }
} 