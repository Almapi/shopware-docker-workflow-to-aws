# shopware-docker-workflow-to-aws

### Todo
multitarget and multistage 



## What is this project about?
This is not a project to develop something. I have created it, to get the idea or inspiration of a Workflow for Docker images, development, CI/CD and deployment


## In Short
The operations team creates a *base image* docker image with the production necessary requirements.
The developer team develop works with the *base image* + DEV ENV Variables

The production ready container is the combination of the following:
 - *base image* 
 - PROD ENV Variables
 - code from the development out of the "app" folder


## Rules for the *base image*
- It **always** consists of a **official** docker hub container ([npm](https://hub.docker.com/_/node)[nginx](https://hub.docker.com/_/nginx), [php](https://hub.docker.com/_/php), [Apache](https://hub.docker.com/_/httpd))
- Never use the **latest** tag from docker hub
- Operation can customize this image and upload it to a private docker repository
- Even if there are zero changes in the image it is get uploaded to a private docker repository

## "app" folder
It contains all project files and the buisness logic. No app related code should be in the root directory

## config folder (recommended)
Create a config folder for you specific configs. Try to separate the prod, stage and dev configs and get a better overview.


## The complete Workflow

### Process of changes in a Base Image (e.g a new php module is necessary or updates in the nginx configs are necessary)
  1. Download official (nginx/php) image from docker
  2. Customize this image for your specific requirements and the specific project (e.g Dockerfile changes FROM nginx:1.19.10 *to* FROM nginx:1.20.0)
  3. Operations Push the new code to the repository 
  4. The Operations Pipeline starts automatically and the new *base image* gets build
  5. Basic CI Tests on the *base images*
  6. Pipeline uploads the *base image* to private docker hub or another private docker repository

### Development
  1. Developer gets automatically informed via Email,Slack,Teams that a new image is ready to pull
  2. Developer stop working with his actual development
  3. Stop the container setup
  4. Download the latest *base image*
  5. Start the container setup  with the new *base image*
  6. They can directly check if their code works with the new *base image*
  7. They can continue and finish the actual develpment task
  8. If the devlopment task is finished they push the Code to the specific branch in git (github, bitbucket) 
  9. The Branch Pipeline starts directly and checks the code with a linter and other custom specific tasks
  10. If the pipeline is finished, the pipeline automatically creates a pull request and informs other developers for code review


### CI 
  1. If the pull request is merged and the pipline detect changes in the specific "app" directory, the production deployment pipeline automatically starts
  2. The *base image* + Stage ENV Variables + the latest code from the "app" directory starts in the staging setup
  3. Other necessary containers (latest database, images, ...) starts on the staging setup
  4. The CI start tests to proove if this container is really production ready
  5. The developer **manually** check the most important functions.
  6. If the CI pipeline is finished, the developer needs to click manually on the "deploy" button to start the deployment

### CD 
  1. The production ready container (with the latest developed code) gets pushed to the AWS ECR 
     (Optionally: In some cases it can be very helpful to also push this image with another tag to your local docker repository)
  3. AWS "Code Pipeline" watches for changes in AWS ECR (You can choose the ECR Repository and a specific tag)
  4. The Pipeline in AWS starts and deploys the container with your configured deployment strategie
  5. Choose the best deployment strategie for docker deployment. Do not use AllAtOnce if you can not handle service interuptions
  6. Now, AWS should automaticallly deploy your container

## Short FAQ?

### How do we add other necessary containers?
If you need this container only for development, it can be added in the docker-compose file for the part development
If you need a container later on producation (redis), you need to inform your Operations Team, because you need redis. They can create a new base image an
 











