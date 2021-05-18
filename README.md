# shopware-docker-workflow-to-aws

## What is this project about?

This is not a project to develop something. I have created it to get the idea of a Workflow for Docker images, development, CI/CD and deployment

## In Short
The operations team creates the docker container out of the production necessary requierements.
The developer team develop works exactly with the *base image* + DEV ENV Variables

The ready for deployment production container is the combination of 
 - *base image* 
 - PROD ENV Variables
 - code from the development


## Rules for the *base image*
- It consists of a **official** docker hub container ([npm](https://hub.docker.com/_/node)[nginx](https://hub.docker.com/_/nginx), [php](https://hub.docker.com/_/php), [Apache](https://hub.docker.com/_/httpd))
- Customize the image and upload it to a private docker repository
- Even if there are zero changes in the image, upload it to a private docker repository

## "app" folder
It contains all project files, buisness logic and requiered for deployments. No app related code should be in the root directory

### The complete Workflow

### Process of changes in a Base Image (e.g a new php module is necessary or updates in the nginx are necessary)
  1. Download a *base (nginx/php) image* from docker
  2. Customize this image for your specific requirements and the specific project
  3. Operations Push the new code to the repository
  4. The Pipeline starts automatically
  5. Basic CI Tests on the *base images*
  6. Pipeline uploads the *base image* to a docker hub or another private docker repository

### Development Image
  1. Developer get automatically informed via Email,Slack,Teams that a new image is ready
  2. Developer stop working with his actual development
  3. Download the latest created image
  4. They can also directly check if the code works with the new latest image 
  5. Continue the actual develpment task
  6. If the devlopment task is finished they push the Code to the specific branch in git (github, bitbucket) 
  7. Pipeline starts directly and checks the code with a linter and other custom specific tasks
  8. If the pipeline is finished, it automatically creates a pull request and informs other developers


### CI 
  1. If the pull request is merged and the pipline detect changes in the specific "app" directory, the production deployment pipeline automatically starts
  2. The *base image* + Stage ENV Variables + the latest code from the "app" directory starts in the staging setup
  3. Other necessary containers (latest database, images, ...) starts on the staging setup
  4. The CI start tests to proove if this container is really production ready
  5. The developer check the most important functions, manually

