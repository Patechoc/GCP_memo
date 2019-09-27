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

- [Module Overview video: Interconnecting Networks](https://www.coursera.org/learn/gcp-infrastructure-scaling-automation/lecture/Hi6hM/module-overview)
- [Module Review video](https://www.coursera.org/learn/gcp-infrastructure-scaling-automation/lecture/BDnmM/module-review)  

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

<img src="../images/Cloud_Interconnect_and_Peering.png"
    alt="Cloud_Interconnect_and_Peering.png"
    style="float: left; margin-right: 10px;" />


 Now as I just explained earlier, Google also offers its own Virtual Private Network service called **Cloud VPN**. This service uses the public Internet but traffic is encrypted and provides access to internal IP addresses. That's why Cloud VPN is a useful addition to Direct Peering and Carrier Peering. 


#### Cloud Interconnect

[video](https://www.coursera.org/learn/gcp-infrastructure-scaling-automation/lecture/OeoTK/cloud-interconnect)

##### Dedicated Interconnect

[Documentation](https://cloud.google.com/interconnect/docs/concepts/dedicated-overview#redundancy)

<img src="../images/Cloud_Interconnect_and_Peering_dedicated_interconnect.png"
    alt="Cloud_Interconnect_and_Peering_dedicated_interconnect.png"
    style="float: left; margin-right: 10px;" />

Definition **SLA***: Service Level Agreement

"99.9% uptime SLA"

**Dedicated Interconnect** needs **Colocation Facility Locations**:

[Documentation on Colocation Facility Locations](https://cloud.google.com/interconnect/docs/concepts/colocation-facilities)

Google locations for physically interconnecting between Google's network and a private network:

<img src="../images/Cloud_Interconnect_physical_Google_location_for_physically_interconnecting_with_private_network.png"
    alt="Cloud_Interconnect_physical_Google_location_for_physically_interconnecting_with_private_network.png"
    style="float: left; margin-right: 10px;" />

#### Supported Service Providers

[Documentation & list of Service providers per country](https://cloud.google.com/interconnect/docs/concepts/service-providers)


#### Partner Interconnect 

If you are no-where near one of this "Colocation Facility Locations", consider **Partner Interconnect**.

[Documentation](https://cloud.google.com/interconnect/docs/concepts/partner-overview#redundancy)

<img src="../images/Cloud_Interconnect_partner_interconnect.png"
    alt="Cloud_Interconnect_partner_interconnect.png"
    style="float: left; margin-right: 10px;" />


#### Comparison of capacity/location's requirements

<img src="../images/Cloud_Interconnect_comparison.png"
    alt="Cloud_Interconnect_comparison.png"
    style="float: left; margin-right: 10px;" />


### Cloud Peering service

- [video](https://www.coursera.org/learn/gcp-infrastructure-scaling-automation/lecture/4QMn8/peering)
- [Documentation on Peering](https://peering.google.com/#/options/peering)
- [Google LLC PeeringDB](https://www.peeringdb.com/asn/15169)
- [Public Peering Exchange Points](https://www.peeringdb.com/net/4319)
- [Peering Service providers](https://cloud.google.com/interconnect/docs/how-to/carrier-peering#service_providers)

There is 2 kinds of Peering:

- Direct Peering 
- Carrier Peering


#### Direct Peering 

<img src="../images/Cloud_Peering_Direct_peering.png"
    alt="Cloud_Peering_Direct_peering.png"
    style="float: left; margin-right: 10px;" />

<img src="../images/Cloud_Peering_Direct_peering_edge_points.png"
    alt="Cloud_Peering_Direct_peering_edge_points.png"
    style="float: left; margin-right: 10px;" />

#### Carrier Peering

If you are no-where near these Google locations, you might consider "**Carrier Peering**".

<img src="../images/Cloud_Peering_Carrier_peering.png"
    alt="Cloud_Peering_Carrier_peering.png"
    style="float: left; margin-right: 10px;" />

#### Comparison of Peering options

<img src="../images/Cloud_Peering_Carrier_comparison_of_options.png"
    alt="Cloud_Peering_Carrier_comparison_of_options.png"
    style="float: left; margin-right: 10px;" />

#### Choosing a connection

The 5 different ways to connect your infrastructure to GCP:

<img src="../images/Cloud_Interconnect_and_Peering.png"
    alt="Cloud_Interconnect_and_Peering.png"
    style="float: left; margin-right: 10px;" />

<img src="../images/Cloud_Interconnect_5_ways.png"
    alt="Cloud_Interconnect_5_ways.png"
    style="float: left; margin-right: 10px;" />


Another way to organize these sources is by:

- **interconnect services**: Interconnect services provide direct access to RFC1918 IP addresses in your VPC with an SLA.
- and by **peering services**: Peering services in contrast offer access to Google public IP addresses only without an SLA. 

 
<img src="../images/Cloud_Peering_Carrier_choose_a_network_connection.png"
    alt="Cloud_Peering_Carrier_choose_a_network_connection.png"
    style="float: left; margin-right: 10px;" />

#### Flow diagram to decide which connection to choose

<img src="../images/Cloud_Interconnect_decision_diagram.png"
    alt="Cloud_Interconnect_decision_diagram.png"
    style="float: left; margin-right: 10px;" />


### Sharing VPC Networks: Shared VPC and VPC Peering

[video](https://www.coursera.org/learn/gcp-infrastructure-scaling-automation/lecture/d6NEM/shared-vpc-and-vpc-peering)

2 configurations for sharing VPC networks across GCP projects:

- **Shared VPC**: allows you to share a network across several projects in your GCP organization
- **VPC Network Peering**: allows you to configure private communication across projects in same or different organizations.

#### Shared VPC

<img src="../images/Sharing_VPC_networkd_shared_VPC.png"
    alt="Sharing_VPC_networkd_shared_VPC.png"
    style="float: left; margin-right: 10px;" />

#### VPC Network Peering

VPC Network Peering is a decentralized or distributed approach to multiproject networking. Because each VPC network, may remain under the control of separate administrator groups, and maintains its own global firewall, and routing tables.

<img src="../images/Sharing_VPC_networkd_VPC_peering.png"
    alt="Sharing_VPC_networkd_VPC_peering.png"
    style="float: left; margin-right: 10px;" />

#### Comparison Shared VPC vs VPC Peering

<img src="../images/Sharing_VPC_comparison.png"
    alt="Sharing_VPC_comparison.png"
    style="float: left; margin-right: 10px;" />

Differences in Network administration models:

<img src="../images/Sharing_VPC_comparison_network_administration_models.png"
    alt="Sharing_VPC_comparison_network_administration_models.png"
    style="float: left; margin-right: 10px;" />








## Load Balancing & Autoscaling  (Module 2)

- [Module overivew video](https://www.coursera.org/learn/gcp-infrastructure-scaling-automation/lecture/hYGrW/module-overview)

## Infrastructure Automation  (Module 3: Deployment Manager & Terraform)

= "Deployment Manager" or "Terraform" to automate the deployment of **GCP infrastructure Services**

Different types of load balancers that can be divided into two categories:

- **global**
- **regional**

<img src="../images/Load_balancing_different_kinds.png"
    alt="Load_balancing_different_kinds.png"
    style="float: left; margin-right: 10px;" />

### Global load balancers

The global load balancers are the HTTP, HTTPS, SSL proxy, and TCP proxy load balancers. 

These load balancers leverage the Google front ends which are software defined, distributed systems that sit in Google's Point-of-Presence and are distributed globally. Therefore, you want to use a global load balancer when your users and instances are globally distributed. Your users need access to the same application and content and you want to provide access using a single anycast IP address.

### Regional load balancers

The regional load balancers are the **internal and network load balancers** and they distribute traffic to instances that are in a single GCP region.

The internal load balancer uses Andromeda which is GCP's software defined network virtualization stack and the network load balancer uses Maglev which is a large distributed software system. There's also another internal load balancer for HTTP, HTTPS traffic, but it's in beta as of this recording

### Managed Instance Groups

[video](https://www.coursera.org/learn/gcp-infrastructure-scaling-automation/lecture/IbepU/managed-instance-groups)

**Definition**: A managed instance group is a **collection of identical virtual machine instances** that you control as a single entity using an **instance template**.

<img src="../images/Load_balancing_Managed_instances.png"
    alt="Load_balancing_Managed_instances.png"
    style="float: left; margin-right: 10px;" />

You will need to create/use Instance Templates: 

<img src="../images/Load_balancing_instance_template.png"
    alt="Load_balancing_instance_template.png"
    style="float: left; margin-right: 10px;" />

Define "rules" for your instance group:

<img src="../images/Load_balancing_manage_instance_groups.png"
    alt="Load_balancing_manage_instance_groups.png"
    style="float: left; margin-right: 10px;" />

In practice, you are just creating VMs but applying much more rules to that instance group.

#### Autoscaling & Health checks

[video](https://www.coursera.org/learn/gcp-infrastructure-scaling-automation/lecture/IylfJ/autoscaling-and-health-checks)

Based on utilization and your thresholds:

<img src="../images/Load_balancing_manage_instance_groups_autoscaling.png"
    alt="Load_balancing_manage_instance_groups_autoscaling.png"
    style="float: left; margin-right: 10px;" />

How to decide on a threshold?

<img src="../images/Load_balancing_manage_instance_groups_autoscaling_how_to_set_thresholds.png"
    alt="Load_balancing_manage_instance_groups_autoscaling_how_to_set_thresholds.png"
    style="float: left; margin-right: 10px;" />


Another tool: Health Check, similar to Uptime checks in Stackdriver:

<img src="../images/Load_balancing_manage_instance_groups_autoscaling_create_health_check.png"
    alt="Load_balancing_manage_instance_groups_autoscaling_create_health_check.png"
    style="float: left; margin-right: 10px;" />


### HTTP(S) Load Balancing

[overview video](https://www.coursera.org/learn/gcp-infrastructure-scaling-automation/lecture/suxVg/overview-of-http-s-load-balancing)

<img src="../images/Load_balancing_HTTPS_load_balancing.png"
    alt="Load_balancing_HTTPS_load_balancing.png"
    style="float: left; margin-right: 10px;" />


Architecture of an HTTP(S) load balancer

<img src="../images/Load_balancing_HTTPS_architecture.png"
    alt="Load_balancing_HTTPS_architecture.png"
    style="float: left; margin-right: 10px;" />

Backend services provided by a load balancer:

<img src="../images/Load_balancing_HTTPS_recall_of_services.png"
    alt="Load_balancing_HTTPS_recall_of_services.png"
    style="float: left; margin-right: 10px;" />

Example #1: HTTP load balancer on a single global IP ([video](https://www.coursera.org/learn/gcp-infrastructure-scaling-automation/lecture/RodYr/example-http-load-balancer))

<img src="../images/Load_balancing_example.png"
    alt="Load_balancing_example.png"
    style="float: left; margin-right: 10px;" />


Example #2: HTTP load balancer (Content-based load balancer)

- 2 backends service: WEB (`/<whatever>`) or VIDEO (`/video`) traffic
- traffic is split by the URL header, as specified by the URL map

<img src="../images/Load_balancing_example_2.png"
    alt="Load_balancing_example_2.png"
    style="float: left; margin-right: 10px;" />

HTTPS load balancer ([video](HTTP(S) load balancing))

- same structure as an HTTP load balancer
- SSL certificate
- QUIC transport layer protocol

<img src="../images/Load_balancing_httpS.png"
    alt="Load_balancing_httpS.png"
    style="float: left; margin-right: 10px;" />

SSL certificate:

<img src="../images/Load_balancing_httpS_SSL_certificate.png"
    alt="Load_balancing_httpS_SSL_certificate.png"
    style="float: left; margin-right: 10px;" />



### Lab: Configuring an HTTP Load Balancer with Autoscaling

- [intro video](https://www.coursera.org/learn/gcp-infrastructure-scaling-automation/lecture/WBXBJ/lab-intro-configuring-an-http-load-balancer-with-autoscaling)
- [lab notes](../labs/lab_Load_balancer_with_Autoscaling.md)
- [review video](https://www.coursera.org/learn/gcp-infrastructure-scaling-automation/lecture/cF54y/lab-review-configuring-an-http-load-balancer-with-autoscaling)

<img src="../images/lab_Load_balancing_autoscaling.png"
    alt="lab_Load_balancing_autoscaling.png"
    style="float: left; margin-right: 10px;" />

- 2 backends in different regions: us_central_1 & europe_west_1
- demonstrate load balancing
- demonstrate autoscaling

### SSL Proxy/TCP Proxy Load Balancing

### Network Load Balancing

### Internal Load Balancing

### Lab: ...


### Choosing a Load Balancer

## Managed Services  (Module 4)

Other managed services that you would like to leverage in GCP (e.g. MongoDB, Kafka, ...)


















## Resources/Articles


- [Networking in Google Cloud Platform course](https://www.qwiklabs.com/courses/751)