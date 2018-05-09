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
