# Work in progress


# Ansible Playbook for Ansible with Serverless

Example [Ansible](https://github.com/ansible/ansible) playbook for deploying [Serverless](https://github.com/serverless/serverless) service.

Build and deploy playbook example [serverless-deployment-ansible-lite](https://github.com/SC5/serverless-deployment-ansible-lite).

Docker is required for deploying the playbook or alternatively Ansible and all the dependencies defined in Dockerfile.

## Project Structure

* `group_vars` default variables
* `inventories` inventory files and variables for environments (development, production, etc.)
* `roles`
  * `infra` role for infrastructure, vpc, database etc.
  * `service` role for Serverless service
* `scripts` scripts that helps deployment

## Deployment

### Local environment

When deploying from local environment AWS secrets needs to be passed to deployment container, for that e.g. `.deploy.sh` script in the project root with contents of

```
#!/usr/bin/env bash

export AWS_ACCESS_KEY=my-access-key
export AWS_SECRET_KEY=my-secret-key

./scripts/deploy-local.sh

```

might ease up the deployment flow.

1. Build Dockerfile with `./scripts/build-docker.sh`
2. Run `./.deploy.sh`

### Jenkins

When using Jenkins on AWS EC2, the role of the instance needs to have permissions to deploy CloudFormation stacks, create S3 buckets, IAM Roles, Lambda and other services that are used in Serverless service.

```
node {
    stage('Checkout repository') {
        git 'https://github.com/SC5/serverless-deployment-ansible-lite.git'
    }

    stage('Build Docker image') {
       sh "./scripts/build-docker.sh"
    }

    stage('Deploy') {
        sh './scripts/deploy-development.sh'
    }
}
```
