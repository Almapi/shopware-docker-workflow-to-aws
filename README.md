# shopware-docker-workflow-to-aws

## What is this project about?

This is not a project to develop something. I have created it to get the idea of a Workflow for Docker images, development, CI/CD and deployment

### The Workflow

### Process of changes in a Base Image (e.g a new php module is necessary or updates in the nginx are necessary)
  1. Download a base (nginx/php) image from docker
  2. Customize this image for your specific requirements and the specific project
  3. Operation Push the new code to the repository
  4. The Pipeline starts automatically
  5. Basic CI Tests on the base images
  6. Pipeline uploads the base image to a docker hub or another private docker repository

### Development Image
  1. Developer get automatically informed via Email,Slack,Teams that a new image is ready
  2. Developer stop working with his actually development
  3. Download the latest created image
  4. He can directly check if his code works with the latest image 
  5. Continue his actually develpment task
  6. If he is finished with his development task, he pushed the Code to his branch in git (github, bitbucket) 
  7. Pipeline starts directly and check the code with a linter and other custom specific tasks
  8. If the pipeline is finished, it creates automatically a pull request and inform other developers


### Deployment 
  1. If the pull request get merged and the pipline track changes in the directory app, the deployment pipeline automatically started
  2. The C
