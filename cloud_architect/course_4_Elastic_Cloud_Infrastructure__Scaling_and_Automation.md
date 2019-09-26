# Elastic Cloud Infrastructure: Scaling & Automation


## Content

[2-weeks course intro video](https://www.coursera.org/learn/gcp-infrastructure-scaling-automation/lecture/KHRmh/course-introduction)

* [**Interconnecting Networks**](#interconnecting-networks)
* [**Load Balancing &amp; Autoscaling**](#load-balancing--autoscaling)
* [**Infrastructure Automation**](#infrastructure-automation)
* [**Managed Services**](#managed-services)
* [Resources/Articles](#resourcesarticles)



This course builds on the [Essential Cloud Infrastructure: Core Services](./course_3_Essential_Cloud_Infrastructure__Core_Services.md) course and enhances your study of architecting with Compute Engine.

In this course, we start by going over:

- the different options to interconnect networks to enable you to connect your infrastructure to GCP. 
- GCP’s load balancing and autoscaling services, which you will get to explore directly. 
- the infrastructure automation services like **Deployment Manager** and **Terraform**, so that you can automate the deployment of GCP infrastructure services.
- The other managed services that you might want to leverage in GCP. 






## Interconnecting Networks (Module 1)

- [Module Overview: Interconnecting Networks](https://www.coursera.org/learn/gcp-infrastructure-scaling-automation/lecture/Hi6hM/module-overview)   

= different options to interconnect to GCP

### Cloud VPN

[video](https://www.coursera.org/learn/gcp-infrastructure-scaling-automation/lecture/oEOJj/cloud-vpn)

<img src="../images/Cloud_VPN.png"
    alt="Cloud_VPN.png"
    style="float: left; margin-right: 10px;" />

Example:

<img src="../images/Cloud_VPN_example.png"
    alt="Cloud_VPN_example.png"
    style="float: left; margin-right: 10px;" />

Dynamic routes with **Cloud Router**

<img src="../images/Cloud_VPN_dynamic_routes_with_Cloud_Router.png"
    alt="Cloud_VPN_dynamic_routes_with_Cloud_Router.png"
    style="float: left; margin-right: 10px;" />

### lab: Cloud VPN

- [Lab Intro: Virtual Private Networks (VPN)](https://www.coursera.org/learn/gcp-infrastructure-scaling-automation/lecture/Z2dw4/lab-intro-virtual-private-networks-vpn)
- [Lab Review video: Virtual Private Networks (VPN)](https://www.coursera.org/learn/gcp-infrastructure-scaling-automation/lecture/nTXif/lab-review-virtual-private-networks-vpn)


### Cloud Interconnect & Peering

[video](https://www.coursera.org/learn/gcp-infrastructure-scaling-automation/lecture/lhPq3/cloud-interconnect-and-peering) 

There are different **Cloud Interconnect** and **Peering services** available to connect your infrastructure to Google's network.

These services can be split into:
- **dedicated connections** versus **shared connections**: Dedicated connections provide a direct connection to Google's network. But, shared connections provide a connection to Google's network through a partner.
- and **layer 2** verses **layer 3** connections: Layer 2 connections use a **VLAN that pipes directly into your GCP environment**, providing connectivity to internal IP addresses in the RFC 1918 address space. Layer 3 connections provide **access to G Suite services, YouTube and Google Cloud APIs using public IP addresses**. 

The services are:

* **Direct Peering**,
* **Carrier Peering**, 
* **Dedicated Interconnect**, 
* and **Partner Interconnect**.

 Now as I just explained earlier, Google also offers its own Virtual Private Network service called Cloud VPN. This service uses the public Internet but traffic is encrypted and provides access to internal IP addresses. That's why Cloud VPN is a useful addition to Direct Peering and Carrier Peering. Let me explain the Cloud Interconnect and Peering services separately first and then I'll provide some guidance on choosing the right connection.


<img src="../images/Cloud_Interconnect_and_Peering.png"
    alt="Cloud_Interconnect_and_Peering.png"
    style="float: left; margin-right: 10px;" />

#### Cloud Interconnect


### Sharing VPC Networks




















## Load Balancing & Autoscaling  (Module 2)


## Infrastructure Automation  (Module 3: Deployment Manager & Terraform)

= "Deployment Manager" or "Terraform" to automate the deployment of **GCP infrastructure Services**


## Managed Services  (Module 4)

Other managed services that you would like to leverage in GCP (e.g. MongoDB, Kafka, ...)

## Resources/Articles
