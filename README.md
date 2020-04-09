# Project fake-backend : Full integration chain

## Build status (badge)

[![Build Status](http://ec2-54-208-88-125.compute-1.amazonaws.com:8080/buildStatus/icon?job=fack-backend-CICD)](http://ec2-54-208-88-125.compute-1.amazonaws.com:8080/job/fack-backend-CICD/)

## Tools 

* AWS and CloudFormation 
* Docker, Docker-compose 
* Ansible
* Jenkins
* SCM (GitLab)
* Slack

## Project 

### The context
                
The objective of this project is to deploy the web application [battleboat.js](https://github.com/billmei/battleboat "battleboat.js") by creating a complete DevOps-like integration chain. For this application to be functional, it is necessary to deploy a MySQL database (backend) and a frontend.
In this project, the problems of businesses, related to storage, to the control of their data and processes were taken into account.

### Infrastructure

#### Description
 
We wanted to reproduce an enterprise-type infrastructure with 4 servers:
- A master type server which will contain the main applications and tools (GitLab, Jenkins, Ansible, Docker…)
- A build server to build our artifact and do unit tests,
- A staging / pre-production server in order to carry out tests of our artifact in real conditions,
- A production server in order to deploy our web application which can be consumed.

#### Infrastructure Diagram


![infra shcem](https://user-images.githubusercontent.com/58267422/78931750-b0f18700-7aa6-11ea-812f-27b436758366.png)


### Choice and description of tools

+ Infrastructure deployed on the AWS cloud provider thanks to CloudFormation in order to favor IaC.
+ Deployment of a containerized GitLab CE instance and activation of the container registry in order to maintain mastery and control of data.
+ Using Docker to containerize the database as well as the frontend in two different containers to favor agility.
+ Use of Ansible to configure our infrastructure.
+ Implementation of a Gitflow to respect good practices. Creation of two branches:
  + The “master” branch which will be used only to deploy our infrastructure and our application in production,
  + The “dev” branch which will be used to develop the functionalities and carry out the tests.
  + Pull request in order to merge the “dev” branch on the “master” branch
+ Use of Jenkins to orchestrate all stages and set up several pipelines.
+ Use notification space on the Slack collaborative platform to notify us of the state of the pipeline.
+ Generation of badges to inform employees

### Workflow

#### Description

On the **“dev”** branch:

+ Development code update via git,
  + Triggering of the first pipeline thanks to the push trigger and the webhook sent to Jenkins:
  + Analysis / linter and tests of the syntax of GO, Dockerfile and docker-compose files (rest line)
  + Notification on Slack of the result of this pipeline

+ If the pipeline is successful, triggering and automatic execution of another pipeline thanks to the success of the first:
  + Analysis / linter and tests of the syntax of mardown, bash, yml files and also of the Ansible syntax
  + Configuration of the environment on the build server, then build and test our artifact (Docke image) on the server. Push our image on our container registry GitLab. Cleaning up the build environment.
  + On the staging server, configuration of the environment, recovery of the necessary sources and deployment of our application in an environment close to production,
  + Several tests of the proper functioning of the application (web services and database)
  + Notification on Slack of the result of this pipeline

+ If the pipeline is successful, set up a Pull Request for a manager to check all the pipelines and that they are working properly.
The manager decides to accept the Pull Request and therefore merge the "dev" branch on the "master" branch to deploy the application in production.

On the **”Master”** branch:
+ A new pipeline is triggered and executed automatically after the Merge Request:
  + Analysis / linter and tests of the syntax of mardown, bash, yml files and also of the Ansible syntax
  + The build and staging steps are voluntarily forgotten,
  + On the production server, configuration of the environment, recovery of the necessary sources and deployment of our application in the production environment
  + Several tests of the proper functioning of the application in production (web services and database)
  + Notification on Slack of the result of this pipeline.

#### Workflow Diagram

![worflow schem](https://user-images.githubusercontent.com/58267422/78931898-e9916080-7aa6-11ea-8545-419e990b95de.png)

------------

### Technical word

Docker, docker-compose, Ansible, Tags, Playbooks, Roles, Galaxy, Jenkins, Shared-library, Pipeline, Notification, linter

### Reference repository

+ [Source code development](https://github.com/samiamoura/fake-backend-continuousIntegration.git "Source code development")
+ [Shared-library](https://github.com/samiamoura/shared-library.git "Shared-library")
