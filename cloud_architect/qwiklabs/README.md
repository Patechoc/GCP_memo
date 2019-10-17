# Cloud Architecture

https://google.qwiklabs.com/quests/24

This fundamental-level quest is unique amongst the other Qwiklabs offerings. The labs have been curated to give IT professionals hands-on practice with topics and services that appear in the [Google Cloud Certified Professional Cloud Architect](https://cloud.google.com/certification/cloud-architect) Certification. From IAM, to networking, to Kubernetes engine deployment, this quest is composed of specific labs that will put your GCP knowledge to the test. Be aware that while practice with these labs will increase your skills and abilities, we recommend that you also review the exam guide and other available preparation resources.


## Prerequisites

This Quest expects familiarity with GCP Services, particularly those in computing and networking. It is recommended that the student have earned a Badge by completing the hands-on labs in the [Baseline: Infrastructure](https://google.qwiklabs.com/quests/33) and/or the [GCP Essentials](https://google.qwiklabs.com/quests/23) Quests before beginning. Additional lab experience with the [Security and Identity Fundamentals](https://google.qwiklabs.com/quests/40) and the [Kubernetes In the Google Cloud](https://google.qwiklabs.com/quests/29) Quests will be useful.

## Quest Outline

* [Orchestrating the Cloud with Kubernetes](https://google.qwiklabs.com/focuses/557?parent=catalog): In this lab you will learn how to: Provision a complete Kubernetes cluster using Google Container Engine; Deploy and manage Docker containers using kubectl; and Break an application into microservices using Kubernetes’ Deployments and Services. [[Hands-on notes](./Orchestrating_the_Cloud_with_Kubernetes.md)]
* [Deployment Manager - Full Production](https://google.qwiklabs.com/focuses/981?parent=catalog): In this lab you will launch a service using Deployment Manager, and monitor it using Stackdriver. You will set up basic black box monitoring with Stackdriver Dashboard and establish uptime check alert notification to trigger incident response. [[Hands-on notes]](./Deployment_Manager_-_Full_Production.md)
* [Continuous Delivery Pipelines with Spinnaker and Kubernetes Engine](https://google.qwiklabs.com/focuses/552?parent=catalog): Create a Kubernetes Engine cluster, deploy an application, and use Spinnaker to continuously deploy the application when changes are made to the application.
* [Continuous Delivery with Jenkins in Kubernetes Engine](https://google.qwiklabs.com/quests/24): In this lab you will deploy and completely configure a continuous delivery pipeline using Jenkins running on Kubernetes Engine and go through the dev - deploy process.
* [Multiple VPC Networks](https://google.qwiklabs.com/focuses/1230?parent=catalog): In this lab, you create several VPC networks and VM instances and test connectivity across networks. [[Hands-on notes]](./Multiple_VPC_Networks.md)
* [Site Reliability Troubleshooting with Stackdriver APM](https://google.qwiklabs.com/focuses/4186?parent=catalog): The objective of this lab is to familiarize yourself with the specific capabilities of Stackdriver to monitor GKE cluster infrastructure, Istio, and applications deployed on this infrastructure.