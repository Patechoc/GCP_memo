# Configure Secure RDP using a Windows Bastion Host


## Setup

```
export PROJECT_ID=qwiklabs-gcp-00-898a73433dc9
gcloud config set project $PROJECT_ID
gcloud config list project
```

## A new non-default VPC has been created

```
gcloud compute --project=${PROJECT_ID} networks create securenet --subnet-mode=custom
```

## The new VPC contains a new non-default subnet within it

```
gcloud compute --project=${PROJECT_ID} networks subnets create securenet-subnet --network=securenet --region=us-central1 --range=10.130.0.0/20
```

## A firewall rule exists that allows TCP port 3389 traffic ( for RDP )


> Add tcp:80 too!!!!!!!!!!!!!!!!!!!!!!!!

```
gcloud compute --project=${PROJECT_ID} firewall-rules create bastionbost-allow-rdp --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:3389 --source-ranges=0.0.0.0/0 --target-tags=bastion
```

## A Windows compute instance called vm-bastionhost



It has a public ip-address to which the TCP port 3389 firewall rule applies.


Follow the description:

- create a VM with boot disc "Windows 2016 Dataserver (with Desktop support)"
- I choose a larger type than default: `n1-standard-4` in order to get enough RAM.
- I increased the disk from `50G` to `200GB`
- Create 2 network interfaces:
   1. one with your new VPC network "secure-network", with an ephemeral IP address
   2. one with your default VPC network "default", also with an ephemeral IP address
- I would add the target tags `bastion` & `http-server` to allow RDP and http traffic


## A Windows compute instance called vm-securehost

It does **not** have a public ip-address.



Follow the description:

- create a similar VM with boot disc "Windows 2016 Dataserver (with Desktop support)"
- I choose a larger type than default: `n1-standard-4` in order to get enough RAM.
- I increased the disk from `50G` to `200GB`
- Create 2 network interfaces:
   1. one with your new VPC network "secure-network", **WITHOUT** ephemeral IP address
   2. one with your default VPC network "default", also **WITHOUT** ephemeral IP address

> Remove External IP address: select "None" to External IP in subnet.

- I would add the target tags `bastion` & `http-server` to allow RDP and http traffic




## The vm-securehost is running Microsoft IIS web server software.

https://cloud.google.com/compute/docs/tutorials/basic-webserver-iis






## Network/Subnetwork

gcloud compute --project=qwiklabs-gcp-00-898a73433dc9 networks create secure-network --subnet-mode=custom

gcloud compute --project=qwiklabs-gcp-00-898a73433dc9 networks subnets create secure-subnetwork --network=secure-network --region=us-central1 --range=10.130.0.0/20



## Video solution provided by the chatbot

https://drive.google.com/file/d/1SBCRGlS9Vkc-anhfKc9OwZz0DR8IVNpG/view