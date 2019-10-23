# Build and Deploy a Docker Image to a Kubernetes Cluster



## Setup

```
export PROJECT_ID=qwiklabs-gcp-00-9d23702b72be
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

The code was however already present in a bucket, so we just fetch it

```
gsutil cp gs://${BUCKET_NAME}/echo-web.tar.gz .
tar -xzvf echo-web.tar.gz
```

## Build a tagged image 

> Note: In order to ensure accurate lab activity tracking you must use `echo-app` as the container repository image name, call your Kubernetes cluster `echo-cluster`, create Kubernetes cluster in `us-central1-a` zone and use `echo-web` for the deployment name.

```
export IMAGE_NAME=echo-app
export TAG_NAME=v1
docker build . -t ${IMAGE_NAME}:${TAG_NAME}
docker images
```

## Change the tag to follow rules from Google Container Registry

> Note: In order to ensure accurate lab activity tracking you must use `echo-app` as the container repository image name, call your Kubernetes cluster `echo-cluster`, create Kubernetes cluster in `us-central1-a` zone and use `echo-web` for the deployment name.

`docker tag echo-app:${TAG_NAME} gcr.io/${PROJECT_ID}/${IMAGE_NAME}:${TAG_NAME}`

## Push the tagged image to Google Container Registry

`docker push gcr.io/${PROJECT_ID}/${IMAGE_NAME}:${TAG_NAME}`

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

Create the cluster:

```
gcloud container clusters create ${DEPLOYMENT_NAME} --num-nodes 2
```

or if you don't forget to set the machine type: `gcloud container clusters create ${DEPLOYMENT_NAME} --num-nodes 2 --machine-type=n1-standard-2`

Update a `kubeconfig` file with appropriate credentials and endpoint information to point kubectl at a specific cluster in Google Kubernetes Engine:

```
gcloud container clusters get-credentials ${DEPLOYMENT_NAME}
```

By default, credentials are written to `HOME/.kube/config`.


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



Check your compute resources... Not `n1-standard-2` machine types ?!!

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

Delete the old node pool:

```
gcloud container node-pools delete default-pool --cluster ${DEPLOYMENT_NAME}
```

## Deploy the application to the Kubernetes Cluster

### Alternative #1: Using command lines in Cloud Shell


Start running the container from the image:

```
kubectl run ${IMAGE_NAME} --image=gcr.io/${PROJECT_ID}/${IMAGE_NAME}:${TAG_NAME} --port 8000
```


Now create a Kubernetes Service, which is a Kubernetes resource that lets you expose your application (that responds on port 8000) to external traffic that respond to normal web requests on port 80, by running the following `kubectl expose` command:


```
kubectl expose deployment ${IMAGE_NAME} --name ${DEPLOYMENT_NAME} --type=LoadBalancer --port 80 --target-port 8000
```

```
kubectl get services
NAME         TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
echo-web     LoadBalancer   10.59.241.15   <pending>     80:31621/TCP   23s
kubernetes   ClusterIP      10.59.240.1    <none>        443/TCP        9m3s
```

Our service is exposed with the same name as our deployment (i.e. `echo-web`) on port 80.

Wait a few seconds to let GCP generate your external IP...

```
kubectl get services
NAME         TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)        AGE
echo-web     LoadBalancer   10.59.241.15   34.68.153.80   80:31621/TCP   111s
kubernetes   ClusterIP      10.59.240.1    <none>         443/TCP        10m
```

Checking the IP address in your browser (http://34.68.153.80) should produce:

```
Echo Test
Version: 1.0.0
Hostname: echo-app-7b45968cfb-fjhs2
Host ip-address(es): 10.56.1.4
```










## Alternative #2: Using the Web Console

1. from GCR, click on your iamge `echo-app` and deploy it to GKE.
2. Fill in the deployment name (`echo-cluster`) and app name (`echo-app`)
3. in GKE, expose the app:
   - port: **80**
   - targetPort: **8000**
   - protocol: **TCP**
   - Service type: **Load Balancer**
   - Service name: **echo-web**
4. Check the external IP in your browser!


## Alternative #3: Using CloudShell and config files (not working yet.. :( )

> NOT WORKING YET!!!!

If you do not want to add `--namespace=echo-web` to all kubectl commands, you can set this as the default namespace for your current context

```
export NAMESPACE=echo-web
kubectl create namespace ${NAMESPACE}
```

See how it looks:

```
kubectl config get-contexts
```

### Update the deployment config file `manifests/echoweb-deployment.yaml`

it should look like this to refer to the new Docker image:

```
cat manifests/echoweb-deployment.yaml 
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: echo-web
  labels:
    app: echo
spec:
  replicas: 2
  template:
    metadata:
      labels:
        app: echo
        tier: web
    spec:
      containers:
      - name: echo-app
        image: gcr.io/qwiklabs-gcp-00-1e1a0525c636/echo-app:v2
        #image: gcr.io/google-samples/echo-app:1.0
        ports:
        - containerPort: 8000
```

Once you have created the Deployment, you can verify that it was created.

```
kubectl get deployments
NAME       READY   UP-TO-DATE   AVAILABLE   AGE
echo-web   2/2     2            2           9s
```
Once the deployment is created, Kubernetes will create a ReplicaSet for the Deployment. We can verify that a ReplicaSet was created for our Deployment:

```
kubectl get replicasets
NAME                  DESIRED   CURRENT   READY   AGE
echo-web-6579bdb94d   2         2         2       26s
```

Ingress is an extension to the Kubernetes API that encapsulates a collection of rules for routing external traffic to Kubernetes endpoints. On Kubernetes Engine, ingress is implemented with a Cloud Load Balancing instance.

It's time to create a Ingress object and service for our `echo-web` deployment.

Apply a configuration to a resource by filename or stdin. The resource name must be specified. This resource will be created if it doesn't exist yet. To use 'apply', always create the resource initially with either 'apply' or 'create --save-config'.

```
cat manifests/echoweb-ingress-static-ip.yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: echo-web
  annotations:
    kubernetes.io/ingress.global-static-ip-name: echoweb-ip
  labels:
    app: echo
spec:
  backend:
    serviceName: echoweb-backend
    servicePort: 8000
---
apiVersion: v1
kind: Service
metadata:
  name: echoweb-backend
  labels:
    app: echo
spec:
  type: NodePort
  selector:
    app: echo
    tier: web
  ports:
  - port: 8000
    targetPort: 8000
```

```
kubectl apply -f manifests/echoweb-ingress-static-ip.yaml
ingress.extensions/echo-web created
service/echoweb-backend created
```

To identify the external IP address of the load balancer, run the following:

```
kubectl get ingress 
NAME       HOSTS   ADDRESS          PORTS   AGE
echo-web   *       35.244.128.151   80      3m50s
echoweb    *       34.96.70.164     80      28m
```

`echo-web` exposes the service to `35.244.128.151`.

You can test by pasting it in your browser and get back:

```
Echo Test
Version: 1.0.0
Hostname: echo-web-6579bdb94d-587nk
Host ip-address(es): 10.56.2.7
```


Finally expose the service with a Load Balancer after replacing the <YOUR.IP.ADDRESS.HERE> with the one above:

```
cat manifests/echoweb-service-static-ip.yaml
apiVersion: v1
kind: Service
metadata:
  name: echoweb
  labels:
    app: echo
spec:
  selector:
    app: echo
    tier: web
  ports:
  - port: 80
    targetPort: 8000
  type: LoadBalancer
  loadBalancerIP: "YOUR.IP.ADDRESS.HERE"
```

then apply it:

```
kubectl apply -f manifests/echoweb-service-static-ip.yaml
```


### Start a single instance of `web-app` and let the container expose port 80

> Even though the application is configured to respond to HTTP requests on port 8000, you must **configure the service to respond to normal web requests on port 80**. When configuring the cluster for your sample application, call your deployment `echo-web`.

```
kubectl run ${IMAGE_NAME} --image=${IMAGE_NAME}:${TAG_NAME} --port=80
```



## References

- Challenge taken from [Qwiklabs: **Update and Scale Out a Containerized Application on a Kubernetes Cluster**](https://github.com/Patechoc/GCP_memo/blob/master/cloud_architect/lab_graded_ScaleOut_containerized_app_on_GKE.md)
- [Qwiklabs: **Kubernetes Load Balancing v1.5**](https://github.com/Patechoc/GCP_memo/blob/master/labs/Lab_Kubernetes_Load_Balancing.md)
- [Qwiklabs: **Managing Deployments Using Kubernetes Engine**](https://github.com/Patechoc/GCP_memo/blob/master/cloud_architect/qwiklabs/Managing_Deployments_Using_Kubernetes_Engine.md)
- [Qwiklabs: **Site Reliability Troubleshooting with Stackdriver APM**](https://github.com/Patechoc/GCP_memo/blob/master/cloud_architect/qwiklabs/Site_Reliability_Troubleshooting_with_Stackdriver_APM.md)
- [Blog post](https://blog.container-solutions.com/using-google-container-registry-with-kubernetes) on how to use kubernetes in practice
- [Migrating workloads to different machine types](https://cloud.google.com/kubernetes-engine/docs/tutorials/migrating-node-pool)
- [Kubernetes tutorial: **Exposing an External IP Address to Access an Application in a Cluster**](https://kubernetes.io/docs/tutorials/stateless-application/expose-external-ip-address/)