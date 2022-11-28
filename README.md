# Cloud DevOps Engineer Capstone Project

This project represents the successful completion of the last final Capstone project and the Cloud DevOps Engineer Nanodegree at Udacity.
- Github: https://github.com/daigiabao996/project5-udacity
- Docker: hhttps://hub.docker.com/repository/docker/emb1605272/capstone-project
- CircleCI: [![CircleCI](https://circleci.com/gh/circleci/circleci-docs.svg?style=svg)](https://app.circleci.com/pipelines/github/daigiabao996/project5-udacity)
## What did I learn?

In this project, I applied the skills and knowledge I developed throughout the Cloud DevOps Nanodegree program. These include:
- Using Circle CI to implement Continuous Integration and Continuous Deployment
- Building pipelines
- Working with Ansible and CloudFormation to deploy clusters
- Building Kubernetes clusters
- Building Docker containers in pipelines
- Working in AWS

## Application

The Application is based on a python3 script using <a target="_blank" href="https://flask.palletsprojects.com">flask</a> to render a simple webpage in the user's browser (and base on project 4).
A requirements.txt is used to ensure that all needed dependencies come along with the Application.

## Kubernetes Cluster

I used AWS CloudFormation to deploy the Kubernetes Cluster.
The CloudFormation Deployment can be broken down into four Parts:
- **Networking**, to ensure new nodes can communicate with the Cluster
- **Elastic Kubernetes Service (EKS)** is used to create a Kubernetes Cluster
- **NodeGroup**, each NodeGroup has a set of rules to define how instances are operated and created for the EKS-Cluster
- **Switch** is needed to configure and manage the Cluster and its deployments and services. I created two management hosts for extra redundancy if one of them fails.

#### List of deployed Stacks:
![CloudFormation](./screenshot/stacks.png)

#### List of deployed Instances:
![Show Instances](./screenshot/Instances.png)

## CircleCi - CI/CD Pipelines

I used CircleCi to create a CI/CD Pipeline to test and deploy changes manually before they get deployed automatically to the Cluster using Ansible.

#### From Zero to Hero demonstration:

![CircleCi Pipeline](./screenshot/build-success.png)

## Linting using Pylint and Hadolint

Linting is used to check if the Application and Dockerfile is syntactically correct.
This process makes sure that the code quality is always as good as possible.

#### This is the output when the step fails:

![Linting step fail](./screenshot/lint-fail.png)


#### This is the output when the step passes:

![Linting step fail](./screenshot/lint_success.png)

## Upload Docker Image After Successful Build

![Upload Docker](./screenshot/docker.png)

## Access the Application

After the EKS-Cluster has been successfully configured using Ansible within the CI/CD Pipeline, I checked the deployment and service log on pipeline as follows:

```
    ##TASK [Get deployment] **********************************************************
    changed: [54.227.124.8] => {
        "ansible_facts": {
            "discovered_interpreter_python": "/usr/bin/python3"
        },
        "changed": true,
        "cmd": "./bin/kubectl set image deployments/****************-deployment ****************-app=******/****************:2da0e633-42bf-4684-816d-ac359173e306",
        "delta": "0:00:00.930410",
        "end": "2022-11-28 10:07:36.790603",
        "invocation": {
            "module_args": {
                "_raw_params": "./bin/kubectl set image deployments/****************-deployment ****************-app=******/****************:2da0e633-42bf-4684-816d-ac359173e306",
                "_uses_shell": true,
                "argv": null,
                "chdir": "/root",
                "creates": null,
                "executable": null,
                "removes": null,
                "stdin": null,
                "stdin_add_newline": true,
                "strip_empty_ends": true,
                "warn": false
            }
        },
        "msg": "",
        "rc": 0,
        "start": "2022-11-28 10:07:35.860193",
        "stderr": "",
        "stderr_lines": [],
        "stdout": "deployment.apps/****************-deployment image updated",
        "stdout_lines": [
            "deployment.apps/****************-deployment image updated"
        ]
    }

TASK [Get service] *************************************************************
changed: [54.227.124.8] => {
    "changed": true,
    "cmd": "./bin/kubectl get services",
    "delta": "0:00:00.923244",
    "end": "2022-11-28 10:07:45.606117",
    "invocation": {
        "module_args": {
            "_raw_params": "./bin/kubectl get services",
            "_uses_shell": true,
            "argv": null,
            "chdir": "/root",
            "creates": null,
            "executable": null,
            "removes": null,
            "stdin": null,
            "stdin_add_newline": true,
            "strip_empty_ends": true,
            "warn": false
        }
    },
    "msg": "",
    "rc": 0,
    "start": "2022-11-28 10:07:44.682873",
    "stderr": "",
    "stderr_lines": [],
    "stdout": "NAME                       TYPE           CLUSTER-IP       EXTERNAL-IP                                                             PORT(S)        AGE\n****************-service   LoadBalancer   10.100.140.191   a6e3d22241f79425eac805cd4a1225a3-19011954.*********.elb.amazonaws.com   80:31706/TCP   88m\nkubernetes                 ClusterIP      10.100.0.1       <none>                                                                  443/TCP        174m",
    "stdout_lines": [
        "NAME                       TYPE           CLUSTER-IP       EXTERNAL-IP                                                             PORT(S)        AGE",
        "****************-service   LoadBalancer   10.100.140.191   a6e3d22241f79425eac805cd4a1225a3-19011954.*********.elb.amazonaws.com   80:31706/TCP   88m",
        "kubernetes                 ClusterIP      10.100.0.1       <none>  
    ]
} 
```

Public LB DNS: http://a6e3d22241f79425eac805cd4a1225a3-19011954.us-east-1.elb.amazonaws.com/

![Access LB DNS](./screenshot/dns.png)
