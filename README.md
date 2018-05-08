# GCP_memo

Useful commands when using Google Cloud Platform



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
