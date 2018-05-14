# Memo for Google Cloud Platform (GCP)

Useful commands/scripts for use on Google Cloud Platform.

## Create a Virtual Machine from CloudShell

```shell
#!/bin/bash

# Remove "_" from USER because "_" can not be used for instance name
export INSTANCE_NAME=datalabvm-${USER//_/}
export ZONE=us-central1-a
export MACHINE_TYPE=n1-standard-1

echo "Creating instance name=$INSTANCE_NAME in zone=$ZONE of type=$MACHINE_TYPE"
gcloud compute instances create $INSTANCE_NAME \
   --image-family=container-vm --image-project=google-containers \
   --zone $ZONE --machine-type $MACHINE_TYPE \
   --metadata "google-container-manifest=$(cat datalab.yaml)" \
   --scopes cloud-platform
```

## Check your CloudSQL instance from CloudShell

```shell
$ mysql --host=35.234.239.132 --user=root --password
MySQL [(none)]> show databases;
MySQL [(none)]> use recommendation_spark;
MySQL [recommendation_spark]> show tables;
MySQL [recommendation_spark]> DESCRIBE Accommodation;
MySQL [recommendation_spark]> SELECT id, count(*) FROM Accommodation GROUP BY id;
MySQL [recommendation_spark]> SELECT DISTINCT userId, count(*) FROM Rating GROUP BY userId;
MySQL [recommendation_spark]> SELECT count(*) FROM Rating;
MySQL [recommendation_spark]> SELECT * FROM Recommendation LIMIT 5;
MySQL [recommendation_spark]> SELECT count(*) FROM Recommendation;
MySQL [recommendation_spark]> select r.userid, r.accoid, r.prediction, a.title, a.location, a.price, a.rooms, a.rating, a.type from Recommendation as r, Accommodation as a where r.accoid = a.id and r.userid = 10;
```

## Authorizing CloudShell to access a CloudSQL instance

Assuming your CloudSQL instance is named `rentals`:

```shell
#!/bin/bash
gcloud sql instances patch rentals \
    --authorized-networks `wget -qO - http://ipecho.net/plain`/32
```

## Managing Dataproc clusters with the CLI/CloudShell

This command creates a Dataproc cluster named **my-second-cluster** in the **us-central1-a** zone. It creates a master node with 1 CPU and a 50 GB disk and 2 worker nodes with the same resources:

```shell
gcloud dataproc clusters create my-second-cluster --zone us-central1-a \
        --master-machine-type n1-standard-1 --master-boot-disk-size 50 \
        --num-workers 2 --worker-machine-type n1-standard-1 \
        --worker-boot-disk-size 50 
```

## Authorizing Dataproc cluster to access a CloudSQL instance

Assuming that:

* your CloudSQL instance is named `rentals`
* your Dataproc cluster has:
  * name: `cluster-1`
  * located in zone: `us-east1-b`
  * nb. workers: `2`
  
```shell
#!/bin/bash

if [ "$#" -ne 3 ]; then
    echo "authorize_dataproc.sh clustername  zone  numworkers"
    echo "  e.g.  ./authorize_dataproc.sh cluster-1 us-east1-b 2"
    exit
fi

CLUSTER=$1
CLOUDSQL=rentals
ZONE=$2
NWORKERS=$3

machines="$CLUSTER-m"
for w in `seq 0 $(($NWORKERS - 1))`; do
   machines="$machines $CLUSTER-w-$w"
done
echo "Machines to authorize: $machines in $ZONE ... finding their IP addresses"

ips=""
for machine in $machines; do
    IP_ADDRESS=$(gcloud compute instances describe $machine --zone=$ZONE --format='value(networkInterfaces.accessConfigs[].natIP)' | sed "s/\[u'//g" | sed "s/'\]//g" )/32
    echo "IP address of $machine is $IP_ADDRESS"
    if [ -z  $ips ]; then
       ips=$IP_ADDRESS
    else
       ips="$ips,$IP_ADDRESS"
    fi
done

echo "Authorizing [$ips] to access cloudsql=$CLOUDSQL"
```


## Big Data and ML Fundamentals using Datalab

### Work from a given project (billing credentials)

Check that you are already authenticated and that the project is already set to your PROJECT_ID:

```shell
$ gcloud auth list
```

```shell
$ gcloud config list project
[core]
project = api-project-56998XXXXXX

Your active configuration is: [cloudshell-XXXXX]
```

If it is not, you can set it with this command:


```shell
$ gcloud config set project <PROJECT_ID>
```

### Start an instance of Cloud Datalab

Pick a zone in a geographically closeby region.

```shell
$ gcloud compute zones list
NAME                       REGION                   STATUS  NEXT_MAINTENANCE  TURNDOWN_DATE
us-east1-b                 us-east1                 UP
us-east1-c                 us-east1                 UP
us-east1-d                 us-east1                 UP
...

$ datalab create bdmlvm --zone europe-west2-b
...
Creating the network datalab-network
Creating the firewall rule datalab-network-allow-ssh
Creating the disk bdmlvm-pd
Creating the repository datalab-notebooks
...
```

Datalab will take about 5 minutes to start :sleeping:

### Open your "Jupyter" notebooks from Cloud Datalab

From CloudShell, click on the Web Preview icon on the top-left corner of the Cloud Shell ribbon. Switch to port 8081.

![Web Preview](https://run-qwiklab-website-prod.s3.amazonaws.com/instructions/documents/42935/original/img/7eb159ad9b4d3d2d.png)

> Note: The connection to your Datalab instance remains open for as long as the datalab command is active. If the cloud shell used for running the datalab command is closed or interrupted, the connection to your Cloud Datalab VM will terminate.

### Checkout any git repository into Cloud Datalab

In Datalab, click on the icon for "Open ungit" in the top-right ribbon. ![Open ungit](https://run-qwiklab-website-prod.s3.amazonaws.com/instructions/documents/42935/original/img/fc5c4f63c40f83f.png)

In the Ungit window, select the text that reads `/content/datalab/notebooks` and remove the notebooks so that it reads `/content/datalab`, then hit enter. ![Datalab ungit](https://run-qwiklab-website-prod.s3.amazonaws.com/instructions/documents/42935/original/img/18f8d226fe047e84.png)

In the panel that comes up, type the following as the GitHub repository to **Clone from** (e.g. `https://github.com/GoogleCloudPlatform/training-data-analyst`).

Then, click on **Clone repository**.

### Get API key(s) to enable Services [optional] 

Depending on what you develop, you will need to enable Services or APIs. 

From the GCP console menu, select **APIs and services** and select **Library** as submenu.

E.g. In the search box, type **vision** to find the Google **Cloud Vision API** and click on the hyperlink and **Enable** if necessary


If you do not already have an API key, from the GCP console menu, select **APIs and services** and select **Credentials**.

Click the **Create credentials** button and select **API key**. Once created, click close. You will need this API key in the notebook later.


### Invoking ML APIs from Datalab

You can find a demo notebook under [ML APIs](./labs/lab04_BigQuery_ML_APIs).

