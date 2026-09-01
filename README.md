# Jenkins Docker CI/CD Pipeline

## Overview

A Jenkins-based CI/CD pipeline that automates the Docker image build, versioning, tagging, and publishing process.

## Workflow

```text
GitHub Repository
       ↓
     Jenkins
       ↓
 Build Docker Image
       ↓
Version & Tag Image
       ↓
Push Image to Registry
```

## Technologies

* Jenkins
* Docker
* GitHub
* Docker Registry
* Jenkinsfile

## Key Features

* Automated Docker image builds
* Automatic image versioning and tagging
* Automated image push to container registry
* Jenkins Pipeline as Code
* GitHub authentication using PAT
* Secure credential management

## Outcome

The pipeline eliminates manual Docker image versioning and publishing by automating the complete build-to-registry workflow through Jenkins.
