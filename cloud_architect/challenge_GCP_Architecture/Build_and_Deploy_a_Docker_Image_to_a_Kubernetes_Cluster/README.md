# Build and Deploy a Docker Image to a Kubernetes Cluster



## Setup

```
export PROJECT_ID=qwiklabs-gcp-00-d2f9976700c2
gcloud config set project $PROJECT_ID
gcloud config list project
```


## Store code on GCP bucket

```
export PROJECT_NAME=$PROJECT_ID
export STORAGE_CLASS=standard
export BUCKET_LOCATION=us
export BUCKET_NAME=${PROJECT_ID}
gsutil mb -p ${PROJECT_NAME} -c ${STORAGE_CLASS} -l ${BUCKET_LOCATION} -b on gs://${BUCKET_NAME}/
```

## Download the code from the bucket to Cloud Shell

```
gsutil cp gs://${BUCKET_NAME}/echo-web.tar.gz .
tar -xzvf echo-web.tar.gz
```

## Build a tagged image 

> Note: In order to ensure accurate lab activity tracking you must use `echo-app` as the container repository image name, call your Kubernetes cluster `echo-cluster`, create Kubernetes cluster in `us-central1-a` zone and use `echo-web` for the deployment name.

```
export IMAGE_NAME=echo-app
docker build . -t ${IMAGE_NAME}:v2
```

## Change the tag to follow rules from Google Container Registry

> Note: In order to ensure accurate lab activity tracking you must use `echo-app` as the container repository image name, call your Kubernetes cluster `echo-cluster`, create Kubernetes cluster in `us-central1-a` zone and use `echo-web` for the deployment name.

`docker tag echo-app:v2 gcr.io/${PROJECT_ID}/echo-app:v2`

## Push the tagged image to Google Container Registry

`docker push gcr.io/${PROJECT_ID}/echo-app:v2`

## Create a Kubernetes Cluster

> Your test environment is limited in capacity, so you should limit the test Kubernetes Cluster you are creating to just two `n1-standard-2` instances. You must call your cluster `echo-cluster`.

> Note: In order to ensure accurate lab activity tracking you must use `echo-app` as the container repository image name, call your Kubernetes cluster `echo-cluster`, create Kubernetes cluster in `us-central1-a` zone and use `echo-web` for the deployment name.

```
export MY_REGION=us-central1
export MY_ZONE=us-central1-a
export CLUSTER_NAME=echo-cluster
export DEPLOYMENT_NAME=echo-web
```

```
gcloud config set project $PROJECT_ID
gcloud config set compute/region $MY_REGION
gcloud config set compute/zone $MY_ZONE
```

`gcloud container clusters get-credentials ${DEPLOYMENT_NAME}`

```
kubectl get nodes
NAME                                      STATUS   ROLES    AGE     VERSION
gke-echo-web-default-pool-19e4b743-s1gk   Ready    <none>   5m11s   v1.13.10-gke.0
gke-echo-web-default-pool-19e4b743-zzhf   Ready    <none>   5m12s   v1.13.10-gke.0
```

```
kubectl describe nodes | grep -i -A2 -B2 standard
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/fluentd-ds-ready=true
                    beta.kubernetes.io/instance-type=n1-standard-1
                    beta.kubernetes.io/os=linux
                    cloud.google.com/gke-nodepool=default-pool
--
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/fluentd-ds-ready=true
                    beta.kubernetes.io/instance-type=n1-standard-1
                    beta.kubernetes.io/os=linux
                    cloud.google.com/gke-nodepool=default-pool
```

Not `n1-standard-2` machine types!

```
gcloud container node-pools list --cluster ${DEPLOYMENT_NAME}
NAME          MACHINE_TYPE   DISK_SIZE_GB  NODE_VERSION
default-pool  n1-standard-1  100           1.13.10-gke.0
```

So we migrate it:

```
gcloud container node-pools create larger-pool \
  --cluster=${DEPLOYMENT_NAME} \
  --machine-type=n1-standard-2 \
  --num-nodes=2
```

## Deploy the application to the Kubernetes Cluster

kubectl run nginx --image=nginx

# Start a single instance of hazelcast and let the container expose port 5701 .
kubectl run hazelcast --image=hazelcast --port=5701

## References

- Challenge taken from [Qwiklabs: **Update and Scale Out a Containerized Application on a Kubernetes Cluster**](https://github.com/Patechoc/GCP_memo/blob/master/cloud_architect/lab_graded_ScaleOut_containerized_app_on_GKE.md)
- [Qwiklabs: **Kubernetes Load Balancing v1.5**](https://github.com/Patechoc/GCP_memo/blob/master/labs/Lab_Kubernetes_Load_Balancing.md)
- [Qwiklabs: **Site Reliability Troubleshooting with Stackdriver APM**](https://github.com/Patechoc/GCP_memo/blob/master/cloud_architect/qwiklabs/Site_Reliability_Troubleshooting_with_Stackdriver_APM.md)