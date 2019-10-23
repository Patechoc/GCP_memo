# Scale Out and Update a Containerized Application on a Kubernetes Cluster

[url](https://google.qwiklabs.com/focuses/1739?parent=catalog)

You need to update the running echo-app application in the echo-web deployment from the v1 to the v2 code you have been provided with. You must also scale out the application to 2 instances and confirm that they are all running.


## Setup

```
export PROJECT_ID=qwiklabs-gcp-00-69db338c318d
gcloud config set project $PROJECT_ID
gcloud config list project
```


## Build the updated application with a new tag

The updated sample application, including the Dockerfile and the application context files, are contained in an archive called echo-web-v2.tar.gz. The archive has been copied to a Google Cloud storage bucket in your lab project called gs://[PROJECT_ID]. V2 of the application adds a version number to the output of the application.


The source files for the second version of the app are already in a bucket on GCP:

```
gsutil ls gs://${PROJECT_ID}
gs://qwiklabs-gcp-00-69db338c318d/echo-web-v2.tar.gz
```

Download it and build the image:

```
gsutil cp gs://${PROJECT_ID}/echo*.gz .
tar -xzvf echo*.gz

export IMAGE_NAME=echo-app
export TAG_NAME=v2
docker build . -t ${IMAGE_NAME}:${TAG_NAME}
```

Here it is:

```
docker images
REPOSITORY          TAG                 IMAGE ID            CREATED                  SIZE
echo-app            v2                  3d12b63bc934        Less than a second ago   11.4MB
<none>              <none>              0e458be881ed        2 seconds ago            264MB
alpine              latest              965ea09ff2eb        45 hours ago             5.55MB
golang              1.8-alpine          4cb86d3661bf        20 months ago            257MB
```

## Push the image to the Google Container Registry

Your organization uses the Google Container Registry to host Docker images for deployments, and uses the gcr.io Google Container Registry hostname for all projects.

Tag the new image to include the path to Google Container Registry:

```
docker tag echo-app:${TAG_NAME} gcr.io/${PROJECT_ID}/${IMAGE_NAME}:${TAG_NAME}
```

Push the image to GCR:

```
docker push gcr.io/${PROJECT_ID}/${IMAGE_NAME}:${TAG_NAME}
```


## Deploy the new image to GKE

### Alternative #1: using the Console


- in **Kubernetes Engine** > **Workload**
- initiate a **Rolling Update** in the hamburger menu "ACTIONS".
- and replace the path to the image with your new tagged image (e.g. `gcr.io/qwiklabs-gcp-00-69db338c318d/echo-app:v2`)
- scale out the app to 2 replicas by clicking on **Scale** in the hamburger menu "ACTIONS".
- click on **Services & Ingress** to find the public IP of the Load Balancer and checl if the service is updated as it should with version 2.0 (e.g. `104.197.41.85`)

In my case `http://104.197.41.85/` returns:

```
Echo Test
Version: 2.0.0
Hostname: echo-web-57bc7965fb-ts95h
Host ip-address(es): 10.40.0.9
```

By refreshing multiple times, you should observe that the **host ip address** jumps between 2, showing that the Load Balancer plays its role and dispatches the load to both replicas.

### Alternative #2: using the config files and CloudShell


Use the provided config files to setup your deployment/service.

```
cat manifests/echoweb-deployment.yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: echoweb
  labels:
    app: echo
spec:
  template:
    metadata:
      labels:
        app: echo
        tier: web
    spec:
      containers:
      - name: echo-app
        image: gcr.io/google-samples/echo-app:1.0
        # image: gcr.io/google-samples/echo-app:1.0
        ports:
        - containerPort: 8000
```


Modify the file above with the correct path to the image on GCR: 

```
echo gcr.io/${PROJECT_ID}/${IMAGE_NAME}:${TAG_
NAME}
gcr.io/qwiklabs-gcp-00-69db338c318d/echo-app:v2
```

The new deployment manifest looks like: 

```
cat manifests/echoweb-deployment.yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: echoweb
  labels:
    app: echo
spec:
  template:
    metadata:
      labels:
        app: echo
        tier: web
    spec:
      containers:
      - name: echo-app
        image: gcr.io/qwiklabs-gcp-00-69db338c318d/echo-app:v2
        # image: gcr.io/google-samples/echo-app:1.0
        ports:
        - containerPort: 8000
```



## References

- similar Qwiklabs:
   - 1
   - 2