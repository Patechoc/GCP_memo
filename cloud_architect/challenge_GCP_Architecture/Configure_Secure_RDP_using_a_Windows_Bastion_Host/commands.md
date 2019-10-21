# Configure Secure RDP using a Windows Bastion Host


## Setup

```
export PROJECT_ID=qwiklabs-gcp-00-434864696f4a
gcloud config set project $PROJECT_ID
```

## A new non-default VPC has been created

`gcloud compute --project=qwiklabs-gcp-00-434864696f4a networks create securenet --subnet-mode=custom`

## The new VPC contains a new non-default subnet within it

`gcloud compute --project=qwiklabs-gcp-00-434864696f4a networks subnets create securenet-subnet --network=securenet --region=us-central1 --range=10.130.0.0/20`

## A firewall rule exists that allows TCP port 3389 traffic ( for RDP )

`gcloud compute --project=qwiklabs-gcp-00-434864696f4a firewall-rules create bastionbost-allow-rdp --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:3389 --source-ranges=0.0.0.0/0 --target-tags=bastion`

## A Windows compute instance called vm-bastionhost

It exists that has a public ip-address to which the TCP port 3389 firewall rule applies.

`gcloud beta compute --project=qwiklabs-gcp-00-434864696f4a instances create vm-bastionhost --zone=us-central1-a --machine-type=n1-standard-1 --subnet=securenet-subnet --network-tier=PREMIUM --maintenance-policy=MIGRATE --service-account=257303595395-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --tags=bastion,http-server --image=windows-server-2016-dc-core-v20191008 --image-project=windows-cloud --boot-disk-size=32GB --boot-disk-type=pd-standard --boot-disk-device-name=vm-bastionhost --reservation-affinity=any`

`gcloud compute --project=qwiklabs-gcp-00-434864696f4a firewall-rules create securenetwork-allow-http --direction=INGRESS --priority=1000 --network=securenetwork --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server`


## A Windows compute instance called vm-securehost

It exists that does not have a public ip-address

`gcloud beta compute --project=qwiklabs-gcp-00-434864696f4a instances create vm-securehost --zone=us-central1-a --machine-type=n1-standard-1 --subnet=securenet-subnet --network-tier=PREMIUM --maintenance-policy=MIGRATE --service-account=257303595395-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --tags=secure --image=windows-server-2016-dc-core-v20191008 --image-project=windows-cloud --boot-disk-size=32GB --boot-disk-type=pd-standard --boot-disk-device-name=vm-securehost --reservation-affinity=any`

## The vm-securehost is running Microsoft IIS web server software.