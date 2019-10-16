# PCA Prep - Update and Scale Out a Containerized Application on a Kubernetes Cluster

~ 1 hour 10 minutes

## Content

* [Overview](#overview)
   * [Topics tested](#topics-tested)
* [Challenge scenario](#challenge-scenario)
   * [Your challenge](#your-challenge)
      * [Build and deploy the updated application with a new tag](#build-and-deploy-the-updated-application-with-a-new-tag)
         * [Setup GCP](#setup-gcp)
         * [Download &amp; extract the 2nd version of the app](#download--extract-the-2nd-version-of-the-app)
         * [Build the updated application in your local CloudShell](#build-the-updated-application-in-your-local-cloudshell)
      * [Push the image to the Google Container Registry](#push-the-image-to-the-google-container-registry)
         * [Tag your newly built image](#tag-your-newly-built-image)
         * [Push the tagged image to Google Container Registry](#push-the-tagged-image-to-google-container-registry)
         * [Deploy the updated application with its new tag](#deploy-the-updated-application-with-its-new-tag)
         * [Scale out the application to 2 instances &amp; Confirm they are all running](#scale-out-the-application-to-2-instances--confirm-they-are-all-running)
            * [Scale out the application](#scale-out-the-application)
            * [Confirm both instances are running](#confirm-both-instances-are-running)
   * [Troubleshooting](#troubleshooting)


## Overview

This lab is similar to a Challenge Lab in the "Challenge: GCP Architecture" Quest.

For this Challenge Lab, you must complete a series of tasks within a limited time period. Instead of following step-by-step instructions, you'll be given a scenario and task - you figure out how to complete it on your own! An automated scoring system (shown on this page) will provide feedback on whether you have completed your tasks correctly.

To score 100% you must complete all tasks within the time period!

When you take a Challenge Lab, you will not be taught GCP concepts. You'll need to use your advanced Google Compute Engine (GCE) skills to assess how to build the solution to the challenge presented. This lab is only recommended for students who have GCE skills. Are you up for the challenge?

### Topics tested

* Update a docker application and push a new version to a container repository.
* Deploy the updated application version to a Kubernetes cluster.
* Scale out the application so that it is running 2 replicas.

## Challenge scenario

You are taking over ownership of a test environment and have been given an updated version of a containerized test application to deploy. Your systems architecture team has started adopting a containerized micro-service architecture. You are responsible for managing the containerized test web applications. An initial version of a test application, called `echo-app` and packaged as a Docker container image, has already been successfully deployed to a Kubernetes cluster called echo-cluster in a deployment called echo-web.

### Your challenge

You need to update the running `echo-app` application in the `echo-web` deployment from the v1 to the v2 code you have been provided with. You must also scale out the application to 2 instances and confirm that they are all running.

#### Build and deploy the updated application with a new tag

The updated sample application, including the Dockerfile and the application context files, are contained in an archive called `echo-web-v2.tar.gz`. The archive has been copied to a Google Cloud storage bucket in your lab project called `gs://[PROJECT_ID]`. V2 of the application adds a version number to the output of the application.

##### Setup GCP

```
export PROJECT_ID=qwiklabs-XXXXYYYY
gcloud config set project $PROJECT_ID
```

##### Download & extract the 2nd version of the app

1. Download the [compressed file](https://googlecoursera.qwiklabs.com/instructions/169259/download) into your local machine.
2. Create a bucket, you can use the name of the project to easily find a unique name
3. upload the compressed file to the bucket manually or using `gsutil`
4. Download the file from Google Storage to your local Cloudshell session: `gsutil cp gs://qwiklabs-XXXYYY/resource-echo-web-v2.tar.gz`
5. extract the new version: tar -xzvf resource-echo-web-v2.tar.gz

##### Build the updated application in your local CloudShell

Here, at once, we build an image from the local `Dockerfile` found in the path `./`, name it `echo-app` and tag it as `v2`:

`docker build . -t echo-app:v2`

You can list your local images with: `docker images`


#### Push the image to the Google Container Registry

Your organization uses the Google Container Registry to host Docker images for deployments, and uses the `gcr.io` Google Container Registry hostname for all projects. You must push the updated image to the Google Container Registry before deploying it.

##### Tag your newly built image

You need to tag the image first before pushing it (see [Documentation: tag/push to GCR](https://cloud.google.com/container-registry/docs/pushing-and-pulling)):

> Remember to replace the hostname (e.g. `gcr.io`) and/or Google Project ID (e.g. `qwiklabs-XXXYYY`) with your own information.

`docker tag echo-app:v2 gcr.io/qwiklabs-XXXYYY/echo-app:v2`

##### Push the tagged image to Google Container Registry

Then you can push the tagged image to Google Container Registry:

`docker push gcr.io/qwiklabs-XXXYYY/echo-app:v2`

##### Deploy the updated application with its new tag

The app is already deployed and running in v1.

You can check it:

- Find your kubernetes cluster: here named "echo-web"
- Find the **external IP of its load balancer**. You can do it from the Console of through 2 command lines:

```
kubectl get services frontend
curl -ks https://<EXTERNAL-IP>
```


or in one command line:

`curl -ks https://kubectl get svc frontend -o=jsonpath={.status.loadBalancer.ingress[0].ip}"```

You should see something like: 


```
Echo Test
Version: 1.0.0
...
```

Hostname and Host IP won't change here since you have only one POD dedicated to that service.


Then, "deploying the new version" simply comes down to an **update of the deployment**:

> If like me, you are not a fan of `vi`, the default editor for `kubectl edit ...`, then add `KUBE_EDITOR="nano"` or `KUBE_EDITOR="emacs"` in front of your `kubectl` command.



`KUBE_EDITOR="nano" kubectl edit deployment echo-web`

Simply replace the path to the image from `gcr.io/qwiklabs-1234567/echo-app:v1` to `gcr.io/qwiklabs-1234567/echo-app:v2`.

This will automatically trigger an update of the container running on the cluster.

You can check that the new version is running in different ways:

* Check the output of the app (since visible here in the output). Find again the **external IP of its load balancer**. You can do it from the Console of through 2 command lines:

```
kubectl get services frontend
curl -ks https://<EXTERNAL-IP>
```

You should now see something like: 


```
Echo Test
Version: 2.0.0
...
```

* You can also check which image version is used in the Google Cloud Console, under **Kubernetes Engine** / **Workload**. Here you shoud see the full path and tag of the running image stored in Google Container Registry.


##### Scale out the application to 2 instances & Confirm they are all running

###### Scale out the application

Check your existing cluster/deployment:

`kubectl get replicasets` should show you 1 running replica.

`kubectl get deployment` should also show the same.

Simply **increase the number of replicas to 2** in your cluster:

`kubectl scale deployment echo-web --replicas=2`

###### Confirm both instances are running

* Check that both PODs are running with: `kubectl get replicasets`
* You can also check this app with its output, refreshing multiple times the app in your browser should change the hostname and host IP address from time to time, jumping from one POD to the other.


### Troubleshooting

Receiving a 504, Gateway timeout error: This might just indicate that the application hasn't quite initialized yet, but it could also be caused by a mismatch between the default port that is set in the Dockerfile (TCP port 8000) and:

* The choice of application port you configured when deploying the application image, or
* When you configured external access.

**Congratulations!**

You have completed the challenge lab.