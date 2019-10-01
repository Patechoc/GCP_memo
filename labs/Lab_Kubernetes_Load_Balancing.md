# Kubernetes Load Balancing v1.5

- [lab overview video](https://www.coursera.org/learn/gcp-infrastructure-containers-services/lecture/xblOd/lab-kubernetes-load-balancing-overview-and-objectives)
- [lab review video](https://www.coursera.org/learn/gcp-infrastructure-containers-services/lecture/tD9bL/lab-kubernetes-load-balancing-review)

~2 hours

## Overview

Google Kubernetes Engine includes integrated support for (L4) network load balancing. To engage network load balancing, all you need to do is include the field type: LoadBalancer in your service configuration file. Kubernetes Engine will set up and connect the network load balancer to your service.

However, if you want more advanced (L7) load balancing features, including cross-region load balancing or content-based load balancing, you need to integrate your service with the HTTP/HTTPS load balancer provided by Compute Engine.

In the first part of this lab, you configure a cluster with network load balancing. In the second part of this lab, you configure a replicated nginx service. Then you use a Kubernetes extension, called ingress, to expose the service behind an HTTPS load balancer.

### Objectives

In this lab, you learn how to perform the following tasks:

* Create a Kubernetes cluster using Google Kubernetes Engine with the built-in network load balancer
* Deploy nginx into the cluster and verify that the application is working
* Undeploy the application
* Re-deploy the cluster using ingress to connect it to a Google Compute Engine HTTPS load balancer.
* Redeploy and test.


## Task 1: Preparation

### Set gcloud default values

1. In the GCP Console, click Activate Cloud Shell (857dc9d7dd799cb2.png). If prompted, click Start Cloud Shell.

2. Set some environment variables:

```shell
export MY_REGION=us-central1

export MY_ZONE=us-central1-a

export CLUSTER_NAME=httploadbalancer
```

For more information about regions and zones, see: https://cloud.google.com/compute/docs/regions-zones/regions-zones

3. Run the following:

```shell
gcloud config set project $DEVSHELL_PROJECT_ID
gcloud config set compute/region $MY_REGION
gcloud config set compute/zone $MY_ZONE
```

To confirm that the project, zone, and region have been set, run the following:

`gcloud config list`

## Task 2: Create a Kubernetes cluster for network load balancing

1. To create a Kubernetes cluster using Kubernetes Engine, run the following:

`gcloud container clusters create networklb --num-nodes 3`

> Note: the parameter num-nodes 3 tells Kubernetes to start three VMs and configure them as nodes in the cluster.

This takes several minutes to run. It creates Compute Engine instances and configures each instance as a Kubernetes node.

These instances don't include the Kubernetes Master node. In Kubernetes Engine, the Kubernetes Master node is a managed service.

2. To see the newly created instances, in the GCP Console, on the Navigation menu, click Compute Engine > VM instances.

## Task 3: Deploy nginx in Kubernetes

Deploy nginx into the Kubernetes cluster and expose the service.

1. In Cloud Shell, deploy nginx by running the following:

`kubectl run nginx --image=nginx --replicas=3`

Result (do not copy; this is example output):

`deployment.apps "nginx" created`

This creates a replication controller to spin up 3 pods; each pod runs the nginx container.

> Note: The parameter replicas=3 is the action that starts up 3 pods and runs the nginx container on each one.

2. To see the status of the deployment, run the following:

`kubectl get pods -owide`

Result (do not copy; this is example output):

```shell
NAME                     READY     STATUS    RESTARTS   AGE       IP          NODE
nginx-4217019353-7rrck   1/1       Running   0          1m        10.X.X.X   gke-networklb-default-pool-cf87a938-9vqp
nginx-4217019353-bxnlw   1/1       Running   0          1m        10.X.X.X   gke-networklb-default-pool-cf87a938-z8tr
nginx-4217019353-cnbxg   1/1       Running   0          1m        10.X.X.X   gke-networklb-default-pool-cf87a938-206q
```


You can see that each nginx pod is now running in a different node (virtual machine).

After all pods have the Running status, you can then expose the nginx cluster as an external service.

3. To expose the nginx cluster to allow access to the deployment, run the following:

```shell
kubectl expose deployment nginx --port=80 --target-port=80 \
--type=LoadBalancer
```

Result (do not copy; this is example output):

`service "nginx" exposed`

This command creates a network load balancer to load-balance traffic to the three nginx instances.

### Find the network load balancer address

1. Run the following:

`kubectl get service nginx`

Result (do not copy; this is example output):

```shell
NAME      CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
nginx     10.X.X.X   <pending>     80:30443/TCP   15s
```

It may take several minutes to see the value of EXTERNAL_IP. If you see <pending> as the example above shows, retry every minute or so until the value of EXTERNAL_IP is displayed as in the example below.

Result (do not copy; this is example output):

```shell
NAME      CLUSTER-IP      EXTERNAL-IP     PORT(S)        AGE
nginx     10.27.251.137   35.192.159.78   80:30443/TCP   1m
```

You can then visit http://EXTERNAL_IP/ to see the server being served through network load balancing.

## Task 4: Undeploy nginx

Undeploy nginx before moving on to deploy a full stack application. You will delete the service and then delete the deployment.

For more information, see:

- Kubernetes services: https://kubernetes.io/docs/concepts/services-networking/service/

- Kubernetes delete: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete

1. To delete the service, run the following:

`kubectl delete service nginx`

Result (do not copy; this is example output):

`service "nginx" deleted`

2. To delete the replication controller and delete the pods (all of the nginx instances), run the following:

`kubectl delete deployment nginx`

Result (do not copy; this is example output):

`deployment.extensions "nginx" deleted`

3. To delete the cluster, run the following:

`gcloud container clusters delete networklb`

4. When prompted, type Y to continue. Deleting the cluster will take a few minutes.
Result (do not copy; this is example output):

```shell
Deleting cluster networklb...done.
Deleted [https://container.googleapis.com/v1/projects/qwiklabs-gcp-3e0eadb8f5747997/zones/us-east1-c/clusters/networklb].
```

## Task 5: Create the Kubernetes cluster for HTTP load balancing

1. To create the Kubernetes cluster, run the following:

`gcloud container clusters create $CLUSTER_NAME --zone $MY_ZONE`

Creating the clusters will take a few minutes.

Result (do not copy; this is example output):

```shell
Creating cluster httploadbalancer...done.
Created [https://container.googleapis.com/v1/projects/qwiklabs-gcp-3e0eadb8f5747997/zones/us-east1-c/clusters/httploadbalancer].
kubeconfig entry generated for httploadbalancer.
NAME              ZONE           MASTER_VERSION  MASTER_IP       MACHINE_TYPE   NODE_VERSION  NUM_NODES  STATUS
httploadbalancer  us-central1-a  1.7.6-gke.1     35.X.X.X  n1-standard-1  1.7.6         3          RUNNING
```


## Task 6: Create and expose the services

1. To create an instance of the nginx image serving on port 80, run the following:

`kubectl run nginx --image=nginx --port=80`

This command runs an instance of the nginx Docker image, serving on port 80. For more information on the nginx Docker image, see: https://hub.docker.com/_/nginx/

2. To create a Kubernetes Engine service that exposes nginx on each Node's IP at a static port (the NodePort), run the following:

`kubectl expose deployment nginx --target-port=80 --type=NodePort`

When you create a service of type NodePort with this command, Kubernetes Engine makes your service available on a randomly selected high port number (e.g. 32640) on all the nodes in your cluster.

A ClusterIP service, to which the NodePort service will route, is automatically created. You can contact the NodePort service, from outside the cluster but within its VPC, by requesting <NodeIP>:<NodePort>. For more information, see: https://kubernetes.io/docs/concepts/services-networking/service/

## Task 7: Create an ingress object

Ingress is an extension to the Kubernetes API that encapsulates a collection of rules for routing external traffic to Kubernetes endpoints. On Kubernetes Engine, ingress is implemented with a Cloud Load Balancing instance.

You need a configuration file that defines an ingress object and configures it to direct traffic to your nginx server.

### Create a YAML configuration file

1. Use nano to create the configuration file for ingress. Call it: basic-ingress.yaml:

`nano basic-ingress.yaml`

2. Copy the following and paste it in the .yaml file:

```shell
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: basic-ingress
spec:
  backend:
    serviceName: nginx
    servicePort: 80
```

3. Press Ctrl+O, ENTER to save the file, and press Ctrl+X to exit nano.

> The basic ingress configuration file in this lab defines a default backend that directs all traffic to the nginx service on port 80. You can also define rules that direct traffic by host/path to multiple Kubernetes services.

Example (do not copy; this is an example): In the following example, Tomcat is running on 8080 and nginx on 80.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: example-ingress
spec:
  backend:
    serviceName: default-handler
    servicePort: 80
  rules:
  - host: my.app.com
    http:
      paths:
      - path: /tomcat
        backend:
          serviceName: tomcat
          servicePort: 8080
      - path: /nginx
        backend:
          serviceName: nginx
          servicePort: 80
```


### Create the Ingress file, monitor, and verify

1. To create the ingress file, run the following:

`kubectl create -f basic-ingress.yaml`

Ingress creates the HTTP load balancing resources in Compute Engine and connects them to the deployment. It takes a few minutes for the backend systems to pass health checks and begin serving traffic.

2. To monitor the progress, run the following:

`kubectl get ingress basic-ingress --watch`

Wait until all three servers are identified, then press Ctrl+C to exit.

3. To check the service status, run the following:

`kubectl describe ingress basic-ingress`

This gives you a list of the HTTP load balancing resources, the backend systems, and their health status.

4. To identify the external IP address of the load balancer, run the following:

`kubectl get ingress basic-ingress`

Use `curl <IP Address>` or browse to the address to verify that nginx is being served through the load balancer. If you get a server error, wait a minute and refresh the page.

## Task 8: Shut down the cluster

Note: You don't have to shut down the cluster. Qwiklabs will handle that for you. However, if you'd like the experience, here are the steps.

1. To delete the ingress object, run the following:

`kubectl delete -f basic-ingress.yaml`

2. To shut down and delete nginx, run the following:

`kubectl delete deployment nginx`

3. To delete the cluster, run the following:

`gcloud container clusters delete $CLUSTER_NAME`

When prompted, type Y to continue. Deleting the cluster will take a few minutes.

## Task 9: Review

You deployed a container application using Kubernetes and Google Kubernetes Engine. First, you tested the configuration of your application. After you verified that it was working, you undeployed it and then re-deployed it behind a Compute Engine HTTPS load balancer, using the Ingress extension to Kubernetes.

