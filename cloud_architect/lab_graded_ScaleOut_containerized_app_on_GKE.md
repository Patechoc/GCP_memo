# PCA Prep - Update and Scale Out a Containerized Application on a Kubernetes Cluster

~ 1 hour 10 minutes


## Overview

This lab is similar to a Challenge Lab in the "Challenge: GCP Architecture" Quest.

For this Challenge Lab, you must complete a series of tasks within a limited time period. Instead of following step-by-step instructions, you'll be given a scenario and task - you figure out how to complete it on your own! An automated scoring system (shown on this page) will provide feedback on whether you have completed your tasks correctly.

To score 100% you must complete all tasks within the time period!

When you take a Challenge Lab, you will not be taught GCP concepts. You'll need to use your advanced Google Compute Engine (GCE) skills to assess how to build the solution to the challenge presented. This lab is only recommended for students who have GCE skills. Are you up for the challenge?

### Topics tested

* Update a docker application and push a new version to a container repository.
* Deploy the updated application version to a Kubernetes cluster.
* Scale out the application so that it is running 2 replicas.

## Challenge scenario7

You are taking over ownership of a test environment and have been given an updated version of a containerized test application to deploy. Your systems architecture team has started adopting a containerized micro-service architecture. You are responsible for managing the containerized test web applications. An initial version of a test application, called `echo-app` and packaged as a Docker container image, has already been successfully deployed to a Kubernetes cluster called echo-cluster in a deployment called echo-web.

### Your challenge

You need to update the running `echo-app` application in the `echo-web` deployment from the v1 to the v2 code you have been provided with. You must also scale out the application to 2 instances and confirm that they are all running.

#### Build and deploy the updated application with a new tag

The updated sample application, including the Dockerfile and the application context files, are contained in an archive called `echo-web-v2.tar.gz`. The archive has been copied to a Google Cloud storage bucket in your lab project called `gs://[PROJECT_ID]`. V2 of the application adds a version number to the output of the application.

#### Push the image to the Google Container Registry

Your organization uses the Google Container Registry to host Docker images for deployments, and uses the `gcr.io` Google Container Registry hostname for all projects. You must push the updated image to the Google Container Registry before deploying it.

### Troubleshooting

Receiving a 504, Gateway timeout error: This might just indicate that the application hasn't quite initialized yet, but it could also be caused by a mismatch between the default port that is set in the Dockerfile (TCP port 8000) and:

* The choice of application port you configured when deploying the application image, or
* When you configured external access.

**Congratulations!**

You have completed the challenge lab.