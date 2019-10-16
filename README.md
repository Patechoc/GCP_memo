# Google CLoud Platform (GCP) Data Engineering & Cloud architect

## Content

* [Cloud Shell commands](#cloud-shell-commands)
* [Data Engineering on GCP Specialization](#data-engineering-on-gcp-specialization)
    * [Objectives](#objectives)
    * [Lectures &amp; labs](#lectures--labs)
* [Cloud Architect on GCP Specialization](#cloud-architect-on-gcp-specialization)
    * [Objectives](#objectives-1)
    * [Lectures &amp; labs](#lectures--labs-1)
* [Codelabs](#codelabs)



## Cloud Shell commands

Cloud shell & `gcloud` commands [memo](./Cloud_Shell_gcloud.md)

## Data Engineering on GCP Specialization


(A set of [courses](https://www.coursera.org/specializations/gcp-architecture) by COURSERA)

### Objectives

This course teaches participants the following skills:

* Design and build data processing systems on Google Cloud Platform
* Leverage unstructured data using Spark and ML APIs on Cloud Dataproc
* Process batch and streaming data by implementing autoscaling data pipelines on Cloud Dataflow
* Derive business insights from extremely large datasets using Google BigQuery
* Train, evaluate and predict using machine learning models using Tensorflow and Cloud ML
* Enable instant insights from streaming data



This class is intended for experienced developers who are responsible for managing big data transformations including:

* Extracting, Loading, Transforming, cleaning, and validating data
* Designing pipelines and architectures for data processing
* Creating and maintaining machine learning and statistical models
* Querying datasets, visualizing query results and creating reports


### Lectures & labs

The courses are:

1. course 1: **Google Cloud Platform** Big Data and Machine Learning Fundamentals
1. course 2: [Leveraging Unstructured Data with Cloud **Dataproc** (managed Apache Spark) on Google Cloud Platform](./data_engineering/Introduction_to_GCP_for_BigData_and_ML.md)
1. course 3: [Serverless Data Analysis with Google **BigQuery** and Cloud **Dataflow**](./data_engineering/BigQuery.md)
1. course 4: [Serverless Machine Learning with **Tensorflow on Google Cloud Platform**](./data_engineering/ML_on_GCP_with_Datalab_and_TensorFlow.md)
1. course 5: [Building Resilient **Streaming Systems** on Google Cloud Platform](./data_engineering/streaming_pipelines_on_GCP.md)



* [Video SUMMARY of all GCP tools](https://www.coursera.org/learn/building-resilient-streaming-systems-gcp/lecture/YUGGw/summary-of-data-engineering-on-gcp-specialization)
* [GCP Cheatsheets](./GCP_cheatsheets.md)



## Cloud Architect on GCP

A Professional Cloud Architect enables organizations to leverage Google Cloud technologies. With a thorough understanding of cloud architecture and Google Cloud Platform, this individual can design, develop, and manage robust, secure, scalable, highly available, and dynamic solutions to drive business objectives.

The Google Cloud Certified - Professional Cloud Architect exam assesses your ability to:

* Design and plan a cloud solution architecture
* Manage and provision the cloud solution infrastructure
* Design for security and compliance
* Analyze and optimize technical and business processes
* Manage implementations of cloud architecture
* Ensure solution and operations reliability

### Useful links and learning pathways

- The [Cloud Architect certification](https://cloud.google.com/certification/cloud-architect) (training, exam's guide, hands-on, ...)
- Training:
   - a set of courses  by COURSERA: [Architecting with Google Compute Engine Specialization](https://www.coursera.org/specializations/gcp-architecture)
   - [Preparing for the Professional Cloud Architect Examination](https://google.qwiklabs.com/courses/879&utm_source=cloud-dot-google&utm_medium=website) & [**[Notes]**](./cloud_architect/Preparing_for_the_Professional_Cloud_Architect_Examination.md)
- Hands-on practice:
   - [**Extra Qwiklabs** for Cloud Architecture](./cloud_architect/qwiklabs/README.md):
      - [Cloud architecture quest](https://google.qwiklabs.com/quests/24?utm_source=gcp&utm_medium=site&utm_campaign=certification)
         1. [Orchestrating the Cloud with Kubernetes](./cloud_architect/qwiklabs/Orchestrating_the_Cloud_with_Kubernetes.md)
         1. Deployment Manager - Full Production
         1. Continuous Delivery with Jenkins in Kubernetes Engine
         1. Continuous Delivery Pipelines with Spinnaker and Kubernetes Engine
         1. [Managing Deployments Using Kubernetes Engine](./cloud_architect/qwiklabs/Managing_Deployments_Using_Kubernetes_Engine.md)
         1. Multiple VPC Networks
         1. Site Reliability Troubleshooting with Stackdriver APM
   - [**Extra Challenge for GCP Architecture**](./cloud_architect/challenge_GCP_Architecture/README.md)
        1. Google Cloud Essential Skills: Challenge Lab
        1. Deploy a Compute Instance with a Remote Startup Script
        1. Configure a Firewall and a Startup Script with Deployment Manager
        1. Configure Secure RDP using a Windows Bastion Host
        1. Build and Deploy a Docker Image to a Kubernetes Cluster
        1. Scale Out and Update a Containerized Application on a Kubernetes Cluster
        1. Migrate a MySQL Database to Google Cloud SQL
- [**Practice exam**](https://cloud.google.com/certification/practice-exam/cloud-architect)


### Architecting with Google Compute Engine Specialization

This course is aimed at IT professionals who are tasked with building and managing solutions in the cloud. If you are a system administrator, system operator, cloud solution architect, or software developer (or want to become one), this course is a great place for you to start.

If you are a data analyst, data engineer, or data scientist (or want to become one), consider starting instead with [GCP Big Data & Machine Learning Fundamentals](https://www.coursera.org/learn/gcp-big-data-ml-fundamentals).

The Professional Cloud Architect certification demonstrates your mastery of developing robust and secure cloud solutions that are aligned to business objectives:

* Plan a cloud solution architecture
* Manage and provision a solution infrastructure
* Design for security and compliance
* Analyze and optimize technical and business processes
* Manage implementation
* Ensure solution and operations reliability


#### Lectures & labs

[**Cloud Services in a table**](./cloud_architect/cloud_services_in_a_table.md)

The courses are:

1. [course 1](https://www.coursera.org/learn/gcp-fundamentals/home/welcome): **[GCP Fundamentals: Core Infrastructure](./cloud_architect/course_1_Core_Infrastructure.md)**
1. [course 2](https://www.coursera.org/learn/gcp-infrastructure-foundation/home/welcome): **Essential Cloud Infrastructure: [Foundation](./cloud_architect/course_2_Essential_Cloud_Infrastructure__Foundation.md)**
1. [course 3](https://www.coursera.org/learn/gcp-infrastructure-core-services?specialization=gcp-architecture): **Essential Cloud Infrastructure: [Core Services](./cloud_architect/course_3_Essential_Cloud_Infrastructure__Core_Services.md)**
1. [course 4](https://www.coursera.org/learn/gcp-infrastructure-scaling-automation?specialization=gcp-architecture): **Elastic Cloud infrastructure: [scaling/automation](./cloud_architect/course_4_Elastic_Cloud_Infrastructure__Scaling_and_Automation.md)**
1. [course 5](https://www.coursera.org/learn/gcp-infrastructure-containers-services): **Elastic Cloud infrastructure: [containers/services](./cloud_architect/course_5_Elastic_Cloud_Infrastructure_Containers_and_Services.md)**
1. [course 6](https://www.coursera.org/learn/cloud-infrastructure-design-process):
   * **[Reliable Cloud infrastructure: design & process](./cloud_architect/course_6_Reliable_Cloud_Infrastructure_Design_and_Process.md)**
   * **[Reliable Cloud infrastructure: Design for Resiliency, Scalability, and Disaster Recovery](./cloud_architect/course_6_Reliable_Cloud_Infrastructure_Design_for_Resiliency_Scalability_and_Disaster_Recovery.md)**



## Codelabs

https://codelabs.developers.google.com/devoxx

* Getting Started with [Cloud Shell & gcloud](https://codelabs.developers.google.com/codelabs/cloud-shell)
