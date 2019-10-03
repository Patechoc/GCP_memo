# Reliable Cloud Infrastructure: Design and Process

## Content

* [Course Intro](#course-intro)
* [Objectives](#objectives)
* [Modules covered](#modules-covered)
    * [4 parts of this class](#4-parts-of-this-class)
* [Defining the Service](#defining-the-service)
    * [Layered and iterative approach to design used in this training](#layered-and-iterative-approach-to-design-used-in-this-training)
    * [Defining the Service: State and Solution](#defining-the-service-state-and-solution)
        * [Google's common design](#googles-common-design)
    * [Defining the Service: Measurement](#defining-the-service-measurement)
        * [Service Level Indicators (SLI)](#service-level-indicators-sli)
        * [Service Level Objectives (SLO)](#service-level-objectives-slo)
        * [Service Level Agreements (SLA)](#service-level-agreements-sla)
    * [User Persona](#user-persona)
    * [Defining the Service: Gathering Requirements](#defining-the-service-gathering-requirements)
        * [Just ask questions: who, what...](#just-ask-questions-who-what)
        * [Specify constraints: time, data , users](#specify-constraints-time-data--users)
        * [Scaling requirements: backend, frontend](#scaling-requirements-backend-frontend)
        * [Size requirements](#size-requirements)
* [Application: Introducing an example Photo Application service](#application-introducing-an-example-photo-application-service)
    * [Gather requirements](#gather-requirements)
    * [Business logic](#business-logic)
    * [From logic, define Services Levels (SLIs, SLOs)](#from-logic-define-services-levels-slis-slos)
    * [Process: test before &amp; during launch](#process-test-before--during-launch)
* [Lab: use Deployment Manager to create a VM](#lab-use-deployment-manager-to-create-a-vm)
* [Business-logic Layer design](#business-logic-layer-design)
    * [Business-logic Layer: Design Overview](#business-logic-layer-design-overview)
    * [Business-logic Layer: Microservice Architecture](#business-logic-layer-microservice-architecture)
    * [Benefits of microservives](#benefits-of-microservives)
    * [How microservices complicate business logic?](#how-microservices-complicate-business-logic)
    * [When using microservices make sense?](#when-using-microservices-make-sense)
    * [Google's offering for microservices](#googles-offering-for-microservices)
        * [Cloud Functions](#cloud-functions)
        * [Cloud Functions example: translate text in an image](#cloud-functions-example-translate-text-in-an-image)
        * [Cloud Functions example: implementation on GAE &amp; limitations](#cloud-functions-example-implementation-on-gae--limitations)
    * [GCP 12-factor support](#gcp-12-factor-support)
        * [GCP development tools matching 12-factor methodology](#gcp-development-tools-matching-12-factor-methodology)
    * [Mapping computing needs to platform products](#mapping-computing-needs-to-platform-products)
    * [Compute System Provisioning](#compute-system-provisioning)
        * [Prefer &amp; plan for horizontal scaling](#prefer--plan-for-horizontal-scaling)
        * [Pros/Cons of horizontal scaling](#proscons-of-horizontal-scaling)
        * [Horizontal scaling design](#horizontal-scaling-design)
        * [Horizontal scaling tradeoffs: latency, capacity, scalability, cost](#horizontal-scaling-tradeoffs-latency-capacity-scalability-cost)
        * [Design first, dimension later](#design-first-dimension-later)
* [Resources/Articles](#resourcesarticles)





## Course Intro

- course site: https://www.coursera.org/learn/cloud-infrastructure-design-process
- [2-weeks course intro video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/Gdkgd): Architecting with Google Cloud Platform Design and Process class
- [Course overview video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/Cp9Nv/course-overview)


While researching the materials for this class, we learned that expert cloud architects often use complex assemblies in their solutions. The assembly changes many aspects of the design at once, and it's very hard to learn design skills from these complex examples.

**A good analogy is an expert chef**.

The chef is preparing a dish and adds a bowl of something, you ask what it is and the chef responds, "Well, that's just a half dozen herbs and spices I like to use." You can't duplicate the recipe if you don't know what the ingredients are in their proportions. So in this class, we've broken down the process of the experts into clear understandable steps. We've tried to show all the ingredients.

The class uses a **tiered design model to organize the learning**. And along the way, you'll have the opportunity to apply what you learned to example designs. 

Speaking of examples, the example solutions in the class exercises are intended to get you thinking about **design**. There are adequate solutions but not perfect solutions, you'll probably think of ways to improve on these solutions and come up with your own designs that are better and that's exactly what we want you to do, to learn to develop your own designs.

<img src="../images/design_and_process.png"
     alt="design_and_process.png"
     style="float: left; margin-right: 10px;" />


## Objectives

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/Cp9Nv/course-overview)


- GCP design skills
- Process knowledge and skills
- Prepare you to continue learning by exposing you to concepts and models (differents SRE & best practices)
- Introduce Deployment Manager as a Cloud Architect tool (template & design)


Learned in the labs:

<img src="../images/course_6_design_and_process_list_labs.png"
     alt="course_6_design_and_process_list_labs.png"
     style="float: left; margin-right: 10px;" />


## Modules covered

1. **Defining the service** (What is that exact process?)
2. **Business logic layer design** (This can also be done on paper before deploying resources)
3. **Data layer design** (identify the data layer, help identify which services we may utilize)
4. **Presentation layer design** (identify the presentation layer, help identify which services we may utilize)
5. **Design for resiliency, scalability and Disaster recovery (DR)** (Build it stronger)
6. **Design for security** (Build it stronger)
7. **Capacity planning and cost optimization** (How much will this design cost? Help design an acceptable budget)
8. **Deployment, monitoring and alerting, and incident response** (Key if you have/want an automated deployment, to understand how we respond to incidences)


### 4 parts of this class

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/SIBjx/modules-in-this-course)

1. Introduction to **architecting concepts and principles**.
   * Defining the Service

2. The application of those principles to a **real world design**.
   
   You'll begin with an application on a single virtual machine, and through the modules, you'll evolve the design into a sophisticated scalable, resilient, stable, secure solution.
3. A **related application problem**.

   You'll be presented with the problem and challenge to come up with your own design, to think through the design challenge on your own, after, you can compare your design to a standard solution. 
4. Hands on labs with **Google Deployment Manager**.

   Deployment Manager is a template-based infrastructure tool. It enables you to produce identical environments for development, test in production, and it's useful for documenting and testing variations in your designs. So you'll be learning deployment manager as a cloud architect tool.


## Defining the Service

- [video course overview](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/hRsj7/defining-the-service-course-overview)

A step-by-step process so you can learn how to think about cloud design.

### Layered and iterative approach to design used in this training

- [video "Define the Service" process](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/VGUfp/defining-the-service-module-overview)

Each module expands on the previous design, exploring it from a new perspective, evolving it into a scalable, reliable, stable, secure design


After introducing a layered model, the module discusses three concepts that are critical to design and to cloud architecture. These are:

- state,
- measurement,
- and requirements.

<img src="../images/Design_process_rough_design.png"
     alt="Design_process_rough_design.png"
     style="float: left; margin-right: 10px;" />

1. **Design the service... roughly** (we will iterate on it) and involve the team going through that design process.
2. then continue with a **more structured design approach**... iterate!

   - is it handling different scenarios?
   - do we have the proper budget?
   - ...
3. Define measurable
   
   - define measurable instruments that will allow us to define
   - are we succeeding the goals that we've set out?
   - What are exactly our service level objectives?
   - and how do we create service level indicators to measure those?
4. Then we move to our **three-tier architecture**:

    - Presentation layer (Networking)
    - Business logic layer (Compute)
    - Data layer (Storage)

    <img src="../images/Design_process_3-tier_architecture.png"
        alt="Design_process_3-tier_architecture.png"
        style="float: left; margin-right: 10px;" />

5. Resiliency, scalability and disaster recovery

    <img src="../images/Design_process_understand_disaster_resiliency.png"
        alt="Design_process_understand_disaster_resiliency.png"
        style="float: left; margin-right: 10px;" />

6. Security

    <img src="../images/Design_process_security.png"
        alt="Design_process_security.png"
        style="float: left; margin-right: 10px;" />

7. Budget, Capacity planning and cost optimization

    <img src="../images/Design_process_budget.png"
        alt="Design_process_budget.png"
        style="float: left; margin-right: 10px;" />

8. Deployment, monitoring, alerting and incident response

    <img src="../images/Design_process_deployment_types.png"
        alt="Design_process_deployment_types.png"
        style="float: left; margin-right: 10px;" />


**Recency bias**

(always remember to try new instead of re-using old)

<img src="../images/Design_process_Recency_assess.png"
     alt="Design_process_Recency_assess.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/Design_process_summary.png"
     alt="Design_process_summary.png"
     style="float: left; margin-right: 10px;" />


### Defining the Service: State and Solution


[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/6aTsE/defining-the-service-state-and-solution) 

State refers to **any action in the system that depends on the memory of a preceding action**. And the data that must be remembered is called state information.

For example, compare two sets of directions:

1. The first one says, walk south for one block and take the first right.
2. The second one says, turn right one block after you've passed the market.

The second instruction requires you to remember that you've passed the market, so it's **stateful**. Whereas the first one does not require you to remember anything, so it's **stateless**. Whether a system is stateful or stateless has a lot of influence on design. And in particular, you have to decide where to store state information, how to retrieve it, and what to do if it's lost.

<img src="../images/Design_process_statefull_stateless.png"
     alt="Design_process_statefull_stateless.png"
     style="float: left; margin-right: 10px;" />

Analogy with kitchen:

- you could build everything yourself (the protein, the vegetables, make the sauce, ...). That gives you **more control** by centralizing everything and allow you to forsee the problem before it gets to the end state. **Problem is scaling**.
- in a stateless approach, it's easier to handle scaling and distribution.

<img src="../images/Design_process_statefull_stateless_cook_analogy.png"
     alt="Design_process_statefull_stateless_cook_analogy.png"
     style="float: left; margin-right: 10px;" />


So what are your states? What kind of information should be stored? Where can we store them? How can we manage them?

Best state is **NO states**. 

<img src="../images/Design_process_where_storing_information.png"
     alt="Design_process_where_storing_information.png"
     style="float: left; margin-right: 10px;" />

How do dal with states?

<img src="../images/Design_process_deal_with_states.png"
     alt="Design_process_deal_with_states.png"
     style="float: left; margin-right: 10px;" />

**Avoid hotspots**. Instead, push them to a backend.

<img src="../images/Design_process_states_Avoid_hotpsots.png"
     alt="Design_process_states_Avoid_hotpsots.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/Design_process_states_Avoid_hotpsots_push_to_backend.png"
     alt="Design_process_states_Avoid_hotpsots_push_to_backend.png"
     style="float: left; margin-right: 10px;" />

You could use **cache**, but that might become overloaded, so to overcome that,  distribute the states onto several backend with load-balancing.

<img src="../images/Design_process_states_distribute_backend_instead_of_cache.png"
     alt="Design_process_states_distribute_backend_instead_of_cache.png"
     style="float: left; margin-right: 10px;" />


##### Google's common design

This solution is a design process and fits many solutions:

Frontend:

- Cloud DNS
- HTTPS Load balancer before pre-emptible machines and store states in the backend.

Backend:
- backend also stateless, also made of pre-emptible machines, pushing data to "a static **stateful** cluster". This stateful cluster is charted, replicated.


<img src="../images/Design_process_states_design_fitting_many_solutions.png"
     alt="Design_process_states_design_fitting_many_solutions.png"
     style="float: left; margin-right: 10px;" />

### Defining the Service: Measurement

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/qhave/defining-the-service-measurement)


Measurement is the key to creating a stable and manageable system.

The Cloud Architect doesn't just design and implement a solution. There are usually required to stick around for some time after the implementation to stabilize the system and make sure it can be operated and maintained. If you design from the beginning, identifying **indicators to be measured** and **objectives to compare them against**, you'll have the information needed for the stabilization processes that surround the system. 

How you should measure your service and discuss the terms we use to describe those measurements?

As the designer of your service, it's up to you to define **how your service should look and feel to your users**. It's equally important **to establish metrics that tell your users what they can expect from your service in terms of reliability and performance**. We'll call this the service level

<img src="../images/Design_process_Service_level.png"
     alt="Design_process_Service_level.png"
     style="float: left; margin-right: 10px;" />

To define a service level, you need to identify what the users care about.

1. **Service Level Indicators (SLI)**: Our first step is to identify the Service Level Indicators or SLIs.
    For example, the availability of a web page or how fast the system responds when a user clicks a button.
2. **Service Level Objectives (SLO)**: Next, we determined the threshold of pain for each of these SLIs. Meaning at what point the users will decide that the service is working properly.
    We'll call this the Service Level Objective or SLO.
3. **Service Level Agreements (SLA)**: Finally, some services are so critical that a formal business agreement will exist between the provider and the users to guarantee a specific behavior and to compensate the users if the service fails to meet this expectation.

<img src="../images/Design_process_Service_level_agreements.png"
     alt="Design_process_Service_level_agreements.png"
     style="float: left; margin-right: 10px;" />

Chances are that you've seen terms like Service Level Agreement or SLA. Perhaps you've even wonder how these figures are determined or more importantly, how they're monitored. Service levels define how a given service or product should behave. There are a number of factors that go into service levels ranging from availability specifications such as 99.99 percent up time or performance specification such as a minimum response time of one second. Service levels helps set the user's expectation and help keep their expectations in line with the design of the service.

##### Service Level Indicators (SLI)

 SLIs should be directly observable and measurable by the users. SLIs should not be internal system metrics such as server, CPU utilization or the number of server failures. This is done for two reasons:
 1. the user can't directly measure those internal factors because they are on the outside of the system.
 2. utilization isn't really an effective measure of the the usability of a system. Instead, utilization is a metric best used by auto-scalars to maintain a consistent user experience

<img src="../images/Design_process_Example_SLI.png"
     alt="Design_process_Example_SLI.png"
     style="float: left; margin-right: 10px;" />

Example SLI:
- users know the service **functions well** because they do not receive errors
- they know it **performs well** because the latency between services is very quick
- they have no idea of the load of CPUs, and they don't care!


<img src="../images/Design_process_Example_SLI_example.png"
     alt="Design_process_Example_SLI_example.png"
     style="float: left; margin-right: 10px;" />


 To provide an accurate measurement of the latency a user would experience, we need to monitor additional systems. In this case, we need to know the true latency of our application. We must account for the latency of a load balancer and of the data server. Note that in this case, we don't have visibility to the user's Internet connection. So, our SLI will be the end-to-end latency from the front-end web load balancer through our service and then back to the data server

<img src="../images/Design_process_Example_SLI_additional_metrics.png"
     alt="Design_process_Example_SLI_additional_metrics.png"
     style="float: left; margin-right: 10px;" />


##### Service Level Objectives (SLO)

Once we've identified what the users care about, we need to quantify those thresholds of pain. This is represented as the Service Level Objective or SLO. The SLO is a threshold value for an SLI, and the result should be set at the lowest or poorest level of service, where the users will still consider the service to be in good working order. That's to say, the SLO represents the point at which the user would consider opening a support ticket because the service failed to meet his or her expectation.


<img src="../images/Design_process_Example_SLO.png"
     alt="Design_process_Example_SLO.png"
     style="float: left; margin-right: 10px;" />

Determining an SLO:

<img src="../images/Design_process_Determining_SLO.png"
     alt="Design_process_Determining_SLO.png"
     style="float: left; margin-right: 10px;" />


When designing your SLOs, it's important to **put reality above utopia**. While we'd love to have a service that is up 100 percent of the time, that also means that the service can never be updated. Google has adopted an error budget methodology. Instead of assuming that every service will run perfectly, services are given error budgets for the SLOs that operates a bit like spending money. Each service starts with a set amount of errors that are considered normal and expected. Any errors in excess of this budget are considered outages. So, the goal is to perform all the system maintenance and updates within this budget. As the days go by each month, the error budget is replenished, giving the development and operations team the breathing room needed to implement changes and apply updates.

<img src="../images/Design_process_Reality_in_service_levels.png"
     alt="Design_process_Reality_in_service_levels.png"
     style="float: left; margin-right: 10px;" />

 it's important to ensure that all SLOs are based on the user experience, rather than an internal system metric. For example, if a service becomes unavailable at 03:00 AM, but no one notices, perhaps the SLO shouldn't be based on a 24-hour day. 

<img src="../images/Design_process_Determining_SLO_alerts_on_UX.png"
     alt="Design_process_Determining_SLO_alerts_on_UX.png"
     style="float: left; margin-right: 10px;" />

##### Service Level Agreements (SLA)

Going one step further, we have SLAs. As previously mentioned, some services are so critical that the loss of the service could represent loss of money for the customer or even loss of life in extreme situations such as, autonomous cars or air traffic control. These contracts define an even more restrictive level of service that is lower or poorer than what is defined in the SLO. The key difference here is that the SLO is a soft target used by the owner of the service to set expectations. The SLA is a business contract that grants a user compensation if the service falls below a certain threshold. Therefore, developers and operations team strive to maintain the SLO, which inherently meets the SLA figures.

<img src="../images/Design_process_Example_SLI_SLO_SLA.png"
     alt="Design_process_Example_SLI_SLO_SLA.png"
     style="float: left; margin-right: 10px;" />


An SLA is not the minimum point at which a services is considered usable. There are some services that are so mission-critical, that the service provider must provide a written guarantee, that the service will perform above a certain specification. That specification is the SLA, and breaking the SLA grants compensation from the provider to the users. Keep in mind, not all services have SLAs, but all services should have an SLO. It's also possible that providers might not publicly disclose their SLOs for fear that users might incorrectly associate the SLO with an SLA.

<img src="../images/Design_process_Example_SLA.png"
     alt="Design_process_Example_SLA.png"
     style="float: left; margin-right: 10px;" />


#### User Persona

If you don't know your user, how can you establish what they find important and acceptable? The answer comes from marketing concept used when developing user interfaces. The user persona. A user persona is an abstract representation of how a certain set of users will use your service. For instance, the power user, the casual user, the road where you are in, the inpatient user. User personas can help you determine the SLIs and the SLOs for your service by understanding how each group will actually use your service.

<img src="../images/Design_process_User_persona.png"
     alt="Design_process_User_persona.png"
     style="float: left; margin-right: 10px;" />


### Defining the Service: Gathering Requirements

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/iHvCE/defining-the-service-gathering-requirements)

Requirements or items that are important to the business, business goals. Requirements are related to measurement. For example; one requirement might be that the service is responsive to users. That requirement is embodied in an objective, such as; no more than half second latency in responding to customer request. And the objective is supported by a measurable indicator, such as; measuring round trip time between user request and delivery of a result. This lesson covers gathering requirements and things to consider and discuss that will help identify requirements. 

##### Just ask questions: who, what...
> 
> * Who's the main character?
> * What do they do?
> * What do they look like?
> * Are they old?
> * Do they have problems?
> * ... 

<img src="../images/Design_process_questions_for_gathering_requirements.png"
     alt="Design_process_questions_for_gathering_requirements.png"
     style="float: left; margin-right: 10px;" />

##### Specify constraints: time, data , users
     
<img src="../images/Design_process_questions_for_time_data.png"
     alt="Design_process_questions_for_time_data.png"
     style="float: left; margin-right: 10px;" />

##### Scaling requirements: backend, frontend

<img src="../images/Design_process_questions_scaling_requirements.png"
     alt="Design_process_questions_scaling_requirements.png"
     style="float: left; margin-right: 10px;" />

##### Size requirements

<img src="../images/Design_process_questions_size_requirements.png"
     alt="Design_process_questions_size_requirements.png"
     style="float: left; margin-right: 10px;" />

This can limit your choice in the type of tools, implementation you will have to build.

## Application: Introducing an example Photo Application service

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/YvgUm/introducing-an-example-photo-application-service)

This lesson introduces the Thumbnail photo service, an example application, that will evolve and design throughout the class. This lesson focuses on applying the principles that were learned in the previous lessons in this module. In this case, we're going to introduce a very basic photo service.

<img src="../images/app_photo_service.png"
     alt="app_photo_service.png"
     style="float: left; margin-right: 10px;" />

### Gather requirements

* users
* speed
* resources
* scale
* size
* availability

<img src="../images/app_photo_service_gather_requirements.png"
     alt="app_photo_service_gather_requirements.png"
     style="float: left; margin-right: 10px;" />

### Business logic

<img src="../images/app_photo_service_gather_busines_logic.png"
     alt="app_photo_service_gather_busines_logic.png"
     style="float: left; margin-right: 10px;" />

### From logic, define Services Levels (SLIs, SLOs)

<img src="../images/app_photo_service_SLO.png"
     alt="app_photo_service_SLO.png"
     style="float: left; margin-right: 10px;" />

### Process: test before & during launch

<img src="../images/app_photo_service_tests.png"
     alt="app_photo_service_tests.png"
     style="float: left; margin-right: 10px;" />

Pre-production tests:

* unit tests
* integration tests
* system tests
* stress test

Production tests:

* Rollout (staged) with user
* acceptance, A/B testing


## Lab: use Deployment Manager to create a VM

- [video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/XjMe5/lab-intro-deployment-manager)

- understand YAML templates syntax
- deploy a single appserver with Deployment Manager




## Business-logic Layer design

### Business-logic Layer: Design Overview

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/tVAxY/business-logic-layer-design-overview)

**What is business logic?**

In computer science, it's **the code that implements business rules that determines what happens to data**. In other words, **processing**. I think of business logic in this way.

Consider the transaction of buying an airline ticket. I could buy the ticket at the counter. I could buy the ticket through a travel agent. I could buy the ticket from a kiosk machine at the airport. I could buy the ticket online using a web browser, or I could buy the ticket using an app on my phone. All of these are different front ends or interfaces, but the transaction itself, of purchasing the ticket, remains the same no matter what interface is used. And that, to me, is the business logic. By definition, business logic operates on data. And that means executing code, processing. So for this reason, the business logic layer is also the part of the design process where you'll consider cloud processing options and determine what service the business logic code will use when it needs to run.

**Microservices** are **a specific kind of Service Oriented Architecture**, or **SOA**, that leverages small, stateless processing for improved scalability and resiliency.

Microservices design is a popular approach to applications, and this lesson explores how microservices work and support on the Google Cloud platform for microservices.


In computer software, business logic or domain logic is part of the program that encodes the real-world business rules that determine how data can be created, stored, and changed. 

In this module, we're going to cover:

* **microservice architecture**,
* **how GCP supports 12-factor**,
* **mapping computing needs to platform products**,
* **compute system provisioning**.

We're also going to explore our photo service. We're going to have our first issue. The photo service is slow, so we're going to have to identify what the cause is, and then understand exactly how we're going to redesign for this. We're also going to have a new design challenge, and this is going to have to do with our log aggregation which you're going to see as a result of our photo service. This will be followed up by another lab. This will be our GCP lab deployment manager in package and deploy.

So now we're going to **package**:

* learn how to package our applications
* and deploy those versus just simply deploying applications in their source.

### Business-logic Layer: Microservice Architecture

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/mLIMB/business-logic-layer-microservice-architecture)

Microservices are a specific kind of service-oriented architecture or SOA that leverages small, stateless processing from proof scalability and resiliency.

Microservices design is a popular approach to applications and this lesson explores how microservices work and support on the Google Cloud Platform for microservices.


<img src="../images/Design_process_business_logic_microservices.png"
     alt="Design_process_business_logic_microservices.png"
     style="float: left; margin-right: 10px;" />

#### Benefits of microservives

<img src="../images/Design_process_business_logic_microservices_benefits_drawbacks.png"
     alt="Design_process_business_logic_microservices_benefits_drawbacks.png"
     style="float: left; margin-right: 10px;" />

#### How microservices complicate business logic?

<img src="../images/Design_process_business_logic_microservices_how_much_more_complex.png"
     alt="Design_process_business_logic_microservices_how_much_more_complex.png"
     style="float: left; margin-right: 10px;" />

#### When using microservices make sense?

<img src="../images/Design_process_business_logic_microservices_use_when_make_sense.png"
     alt="Design_process_business_logic_microservices_use_when_make_sense.png"
     style="float: left; margin-right: 10px;" />

#### Google's offering for microservices

##### Cloud Functions

<img src="../images/Design_process_business_logic_microservices_cloud_functions.png"
     alt="Design_process_business_logic_microservices_cloud_functions.png"
     style="float: left; margin-right: 10px;" />


##### Cloud Functions example: translate text in an image

<img src="../images/Design_process_business_logic_microservices_cloud_functions_example.png"
     alt="Design_process_business_logic_microservices_cloud_functions_example.png"
     style="float: left; margin-right: 10px;" />

##### Cloud Functions example: implementation on GAE & limitations

<img src="../images/Design_process_business_logic_microservices_cloud_functions_example_in_GAE.png"
     alt="Design_process_business_logic_microservices_cloud_functions_example_in_GAE.png"
     style="float: left; margin-right: 10px;" />

#### GCP 12-factor support

https://12factor.net/

<img src="../images/Design_process_12-factor.png"
     alt="Design_process_12-factor.png"
     style="float: left; margin-right: 10px;" />

##### GCP development tools matching 12-factor methodology

<img src="../images/Design_process_12-factor_GCP_tools.png"
     alt="Design_process_12-factor_GCP_tools.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/Design_process_12-factor_GCP_tools_2.png"
     alt="Design_process_12-factor_GCP_tools_2.png"
     style="float: left; margin-right: 10px;" />

Storing states in your environment

<img src="../images/Design_process_12-factor_GCP_tools_storing_states.png"
     alt="Design_process_12-factor_GCP_tools_storing_states.png"
     style="float: left; margin-right: 10px;" />

#### Mapping computing needs to platform products

<img src="../images/Design_process_12-factor_where_to_get_CPUs.png"
     alt="Design_process_12-factor_where_to_get_CPUs.png"
     style="float: left; margin-right: 10px;" />

Think of **App Engine** first, then **K8s** if you need more control on the scaling, or **Compute Engine** as a last resort.

<img src="../images/Design_process_12-factor_think_about_appengine_first.png"
     alt="Design_process_12-factor_think_about_appengine_first.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/Design_process_12-factor_think_about_appengine_code_first.png"
     alt="Design_process_12-factor_think_about_appengine_code_first.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/Design_process_12-factor_think_about_k8s_second.png"
     alt="Design_process_12-factor_think_about_k8s_second.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/Design_process_12-factor_think_about_ComputeEngine_third.png"
     alt="Design_process_12-factor_think_about_ComputeEngine_third.png"
     style="float: left; margin-right: 10px;" />


#### Compute System Provisioning

<img src="../images/Design_process_compute_system_provisioning.png"
     alt="Design_process_compute_system_provisioning.png"
     style="float: left; margin-right: 10px;" />

##### Prefer & plan for horizontal scaling

<img src="../images/Design_process_compute_system_provisioning_how_to_grow.png"
     alt="Design_process_compute_system_provisioning_how_to_grow.png"
     style="float: left; margin-right: 10px;" />

##### Pros/Cons of horizontal scaling

<img src="../images/Design_process_compute_system_provisioning_prefer_horizontal_scaling.png"
     alt="Design_process_compute_system_provisioning_prefer_horizontal_scaling.png"
     style="float: left; margin-right: 10px;" />

##### Horizontal scaling design

<img src="../images/Design_process_compute_system_provisioning_horizontal_scaling_design.png"
     alt="Design_process_compute_system_provisioning_horizontal_scaling_design.png"
     style="float: left; margin-right: 10px;" />


##### Horizontal scaling tradeoffs: latency, capacity, scalability, cost

<img src="../images/Design_process_compute_system_provisioning_horizontal_scaling_tradeoffs.png"
     alt="Design_process_compute_system_provisioning_horizontal_scaling_tradeoffs.png"
     style="float: left; margin-right: 10px;" />

##### Design first, dimension later

<img src="../images/Design_process_compute_system_provisioning_design_first_dimension_later.png"
     alt="Design_process_compute_system_provisioning_design_first_dimension_later.png"
     style="float: left; margin-right: 10px;" />


## Application: The photo service is slow!!!

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/rb6kW/the-photo-service-is-slow)

### Business problem

- the service has been slowing down
- what's te cause of this slow service?
- What can be done about it?
- What opportunities does this offer for improving the design?


<img src="../images/app_photo_service_slow.png"
     alt="app_photo_service_slow.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/app_photo_service_problem.png"
     alt="app_photo_service_problem.png"
     style="float: left; margin-right: 10px;" />

### Systematic logical troubleshooting

<img src="../images/app_photo_service_problem_troubleshooting.png"
     alt="app_photo_service_problem_troubleshooting.png"
     style="float: left; margin-right: 10px;" />

### Collaboration & communication

<img src="../images/app_photo_service_problem_troubleshooting_collaboration.png"
     alt="app_photo_service_problem_troubleshooting_collaboration.png"
     style="float: left; margin-right: 10px;" />

### Break down business logic on the photo service

<img src="../images/app_photo_service_slow_breakdown.png"
     alt="app_photo_service_slow_breakdown.png"
     style="float: left; margin-right: 10px;" />

### Identify the attributes of the different services?

<img src="../images/app_photo_service_slow_breakdown_identify_attributes_Services.png"
     alt="app_photo_service_slow_breakdown_identify_attributes_Services.png"
     style="float: left; margin-right: 10px;" />

### Segregate services for better performance and scalability

<img src="../images/app_photo_service_slow_segregates_Services.png"
     alt="app_photo_service_slow_segregates_Services.png"
     style="float: left; margin-right: 10px;" />

### What about our Service Level Objectives (SLOs) and Indicators (SLIs)

<img src="../images/app_photo_service_slow_new_SLO_SLI.png"
     alt="app_photo_service_slow_new_SLO_SLI.png"
     style="float: left; margin-right: 10px;" />

## Design challenge #1: Log aggregation

This lesson introduces a related independent design problem, the **log files**.

On the original single virtual machine solution, all of the log files were stored on the instance. As the design changes, new complications are introduced into the design of a log system. To meet troubleshooting requirements, the separate logs will need to be aggregated onto a single system. Watch the lesson that describes the problem and then come up with your own solution. When you're ready, continue the lesson to see a sample solution. Remember that the sample solution is not the best possible solution, it's just an example, your design might be better


<img src="../images/challenge_log_files.png"
     alt="challenge_log_files.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/challenge_log_files_segregated.png"
     alt="challenge_log_files_segregated.png"
     style="float: left; margin-right: 10px;" />

What that means is, we have two different log entries now, one for the web and one for the application. They do share a common session ID fields so we need to have a process that will join those together for troubleshooting.


 we have logs on two servers, we're going to aggregate them into a single log. So, the solution here is to design a logging server. This server will accept logs, perhaps we're just using sys log fluid d or something else like that, maybe even Kafka. So, in this case we're going to create, let's call this a Python script. We're just going to whip something together, we're going to have a daily cron job that's going to take all these log files and it's going to aggregate them. So, it's purpose is to ingest that data to open the log files together, so it pushes them together. Then we need to transform them. So, even though they're all in one file, we're going to transform them and join them on that session ID. Then we're going to output that data and now we have aggregate log files. Then the daily cron job will go ahead and repeat itself. So, every 24 hours, we will have combined logs. 

<img src="../images/challenge_log_files_segregated_implement_logging_server.png"
     alt="challenge_log_files_segregated_implement_logging_server.png"
     style="float: left; margin-right: 10px;" />

**New business logic:**

<img src="../images/challenge_log_files_segregated_implement_logging_server_new_business_logic.png"
     alt="challenge_log_files_segregated_implement_logging_server_new_business_logic.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/challenge_log_files_slow_new_business_logic_output_numbers_for design.png"
     alt="challenge_log_files_slow_new_business_logic_output_numbers_for design.png"
     style="float: left; margin-right: 10px;" />


## lab: useDeployment Manager: Package and Deploy

In this lab, you'll learn:

- how to customize an instance using the deployment manager template. 
- how to install library packages and software required of an application.
- how to package and install a Python application, the Echo application, that will be installed at boot time and will run when the instance starts up.

The application development team is supposed to provide an early version of the photo application for you to start building the infrastructure, but they're late. Instead, they provide a similar Python application, called Echo for you to use until the real app is ready. Note that you'll not need to do any programming for this lab. The application is already written. You'll just be creating and configuring the templates. Your challenge in this lab is to use deployment manager templates to create the app server. You'll need to package and install the pre-written Python application. You'll need to install the required application frameworks that the application depends on. And you'll need to handle environment metadata to customize and initialize the server during the boot process.


**Qwiklabs – Deployment Manager Package and Deploy**

In this lab you will...

* Develop a service using a pre-written Python application called "Echo" and example Deployment Manager templates written in YAML and JINJA2.
* Create a deployment package suitable for Deployment Manager using the python package manager, pip.
* Stage your package in a Cloud Storage bucket.
* Manually test the application to ensure that it is working properly.
* Use Deployment Manager to deploy the Echo service.
* Test the new service.

- [video lab overview](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/V7NHs/deployment-manager-package-and-deploy)
- [lab notes](../labs/lab_design_and_process_develop_and_deploy_a_service_with_deployment_manager.md)





## Data Layer Design

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/Sw0BJ/data-layer-design-overview)

The data layer covers the storage and retrieval of data, including the mechanisms of storage and retrieval, such as databases and file systems, and including the access methods that use the services, such as SQL and API's. However, what's not covered in this module is the transport of data into, around, and out of the system. The networking transport of data is covered in the presentation layer module.

The data layer includes; the data persistent mechanisms, such as the database services and storage services, and the data access layer, which encapsulates the persistent mechanisms and exposes the data.

<img src="../images/Data_layer_design_data_layer.png"
     alt="Data_layer_design_data_layer.png"
     style="float: left; margin-right: 10px;" />

### Classifying and Characterizing Data

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/2FsRQ/data-layer-design-classifying-and-characterizing-data)

There are many ways to characterize data such as structured or unstructured, persistent or ephemeral. This lesson focuses on characterizing data based on what the user cares about. Most often, the user doesn't care about the underlying technology, but about whether the data is accessible when they need it. And whether the data they retrieve is the same data they stored and not modified or altered.


<img src="../images/dummyData_layer_design_data_integrity.png"
     alt="dummyData_layer_design_data_integrity.png"
     style="float: left; margin-right: 10px;" />

What users really care about is **data integrity**.

They can't really distinguish the difference between:

- data loss,
- data corruption,
- extended unavailability.

If their data's not there, it might as well be gone.

However, as an engineer, persistence and access are very separate. Access, basically, is a loss of data access, is very important to users, but that doesn't mean that our data is gone. Persistence and proactive detection and rapid recovery is really what our goal is. If there is data that is somehow lost we want to be able to get it back, **recover it maybe using version controls or backups**. **Or some kind of failover mechanism**.

<img src="../images/Data_layer_design_data_transaction_property.png"
     alt="Data_layer_design_data_transaction_property.png"
     style="float: left; margin-right: 10px;" />

So when you're thinking about your data, you need to think about what data transaction properties are going to be required. And, of course, you have your menu, and **you can only really pick two**, and this is the **CAP Theorem**. So here you have to choose between:
- **consistency**,
- **availability**, 
- and **partition tolerance**.


#### BASE

If you want availability and partition tolerance, you can go over to BASE, which is **Basically Available Storage**. It's soft state and it's going to be eventual consistency. Now, what that means is, you may make an update, it may not be available there immediately. Or specific changes may not necessarily be available. But with that type of ability, it allows you to write data, it allows you to expand your data very, very quickly. And eventually everything will all be consistent.

#### ACID

Now if you want pure consistency, then we get into what we call ACID transactions. Which stands for **Atomicity, Consistency, Isolation,** and **Durability**. That means if you write some data to your data storage system, it is guaranteed to be there. You will not get an acknowledgement until it has been written and it is completely fault-tolerant, replicated, et cetera. So it will ensure any type of failover capabilities.

#### What are the data consistency requirements?

<img src="../images/Data_layer_design_data_consistency_requirements.png"
     alt="Data_layer_design_data_consistency_requirements.png"
     style="float: left; margin-right: 10px;" />

#### What are you truing to optimize? Domain/App specific

- uptime
- latency
- scale
- velocity
- privacy

<img src="../images/Data_layer_design_data_consistency_requirements_what_to_optimize.png"
     alt="Data_layer_design_data_consistency_requirements_what_to_optimize.png"
     style="float: left; margin-right: 10px;" />

### Data Ingest and Data Migration

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/yWyty/data-layer-design-data-ingest-and-data-migration)

**Data migration** is the idea that data already exists somewhere and that it will be transported into the cloud. Once the base of existing data has been migrated, updates and new data will be added to the cloud version and the original is no longer needed.

**Data ingestion** is the idea that data will continue to originate outside of the cloud and will be periodically loaded into the cloud.

This lesson covers the most **common methods for getting data into the cloud**.

#### GCS migration tools: console, gsutil, JSON API

<img src="../images/Data_layer_design_Data_migration_tools_on_GCP.png"
     alt="Data_layer_design_Data_migration_tools_on_GCP.png"
     style="float: left; margin-right: 10px;" />

About data migration. We need to bring data into our system and we potentially need to migrate it from different locations.

So there's a few ways that we do this:

* One is directly from the **console for GCS**, you can just drag and drop files.
* There's the **`gsutil` for the Google storage utility command**. This also has the built in Amazon S3 SDK so it allows us to transfer data from Amazon buckets on ourselves. But it also gives us capability to do identity access management, sign URLs, access control lists. There's a whole bunch of capabilities just from this command line.
* And then of course, our **JSON API**. So here you can make the API calls yourself, you can compress this data, and you can also do partial uploads which is really good especially because most uploads to GCS buckets are going to be entire updates. And so in this case here, if you control the API, you might be able to have a little bit more efficiency built in.


#### GCS migration tools for large transfers: Cloud Storage Transfer Service

<img src="../images/Data_layer_design_Data_migration_tools_on_GCP_large_trasnfers.png"
     alt="Data_layer_design_Data_migration_tools_on_GCP_large_trasnfers.png"
     style="float: left; margin-right: 10px;" />

Now we also have a commercial service, well not a commercial, but it's a service that is designed for very large transfers, and it's called the **Cloud transfer service**. It's basically a web based interface that's using a lot of the APIs in the back end to transfer data between say, Amazon and us, and it does so very efficiently. Now it usually recommends typically over a terabyte worth of data because it can do a lot of management like deleting the source data, making sure one is synchronized, overriding any changes, but optimizes the number of streams as well. So it looks at that source material says, how many of them are there? I will create an optimized number of streams to ensure we can get this moved over as fast as possible.

#### GCS migration tools for large transfers without network: Google Transfer Appliance

<img src="../images/Data_layer_design_Data_migration_tools_on_GCP_trasnfers_appliance.png"
     alt="Data_layer_design_Data_migration_tools_on_GCP_trasnfers_appliance.png"
     style="float: left; margin-right: 10px;" />

Now of course, the size always does matter and if you need to be transferring hundreds of terabytes of data, perhaps, you don't want to do that over the network. Even though ingress is free, it may not be time efficient. So we now have the transfer appliance very similar to Amazon's Snowball capability. It's an appliance that ships on site, you can either rock it or do it as a off-the-shelf appliance up to a petabyte worth of storage. And this way, you can copy all the data on your local network and then ship it to Google for data ingest.
 
#### GCS migration tools scal-up table

<img src="../images/Data_layer_design_Data_migration_tools_summary_table_scaleup.png"
alt="Data_layer_design_Data_migration_tools_summary_table_scaleup.png"
style="float: left; margin-right: 10px;" />


Here's a fun little table to kind of give you an idea. When you really start to scale up and the amount of data you need to move over to us, you also have to scale up in the amount of bandwidth that you might have capable. Now, even at 10 gigs fully saturated, 30 hours is pretty considerable but, I wouldn't say considerable. The fact that you probably cannot utilize that full amount of bandwidth yourself and this is where you'll start to see some serious delays in a larger data transfer. So just kind of use this and we do recommend using the Google transfer appliance probably between 10 and 100 terabytes just to make your life a little easier.

#### Data ingestion tools

<img src="../images/Data_layer_design_Data_ingestion_tools.png"
     alt="Data_layer_design_Data_ingestion_tools.png"
     style="float: left; margin-right: 10px;" />

  
So other ways that we ingest data into your service.

* Now first, obviously, it can be done **through the network** which is going to be, its globally available, it's highly available, there's a bandwidth of a 100 gigs and many of our peering locations in low latency.
* You can post directly **to cloud storage using HTTP, RESTful APIs**.
* You can put front end application **through App Engine**. So this way, we can build an authentication, integrate that way, even cloud endpoints.

So we can expose just a simple API that can ingest data into your application even through cloud functions, or set up a computer engine instance and store it on local persistent disk. There's a number of different ways to bring data in, some of them have various pros and cons which we can cover.


### Identification of Storage Needs and Mapping to Storage Systems

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/dQlqJ/data-layer-design-identification-of-storage-needs-and-mapping-to-storage-systems)

Google Cloud Platform offers a wide variety of storage and database services.

This lesson provides a general **method to narrow down the list of candidates storage services**. The diagrams and charts in this lesson can help you ask meaningful questions about the data that will help you match the data to storage services.

#### Choose a storage solution between: DISK, MOBILE or CLOUD solutions

<img src="../images/Data_layer_design_GCP_solution_Disk-or-Mobile-or-Cloud.png"
     alt="Data_layer_design_GCP_solution_Disk-or-Mobile-or-Cloud.png"
     style="float: left; margin-right: 10px;" />

#### Choose a storage solution for unstructured data on Cloud Storage: regional, multi-regional, nearline, coldline

<img src="../images/Data_layer_design_Coud_Storage_regional_coldline_nearline.png"
     alt="Data_layer_design_Coud_Storage_regional_coldline_nearline.png"
     style="float: left; margin-right: 10px;" />

#### Choose a storage solution for analytics: BigQuery

<img src="../images/Data_layer_design_Why_choose_BigQuery.png"
     alt="Data_layer_design_Why_choose_BigQuery.png"
     style="float: left; margin-right: 10px;" />

#### Choose a storage solution for SQL (scalable or not): Clud SQL or Spanner

<img src="../images/Data_layer_design_Why_choose_if_SQL.png"
     alt="Data_layer_design_Why_choose_if_SQL.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/Data_layer_design_Why_choose_if_SQL_scalable.png"
     alt="Data_layer_design_Why_choose_if_SQL_scalable.png"
     style="float: left; margin-right: 10px;" />

#### Choose a storage solution for NoSQL: Cloud Datastore

<img src="../images/Data_layer_design_Why_choose_if_NoSQL.png"
     alt="Data_layer_design_Why_choose_if_NoSQL.png"
     style="float: left; margin-right: 10px;" />

#### Choose a storage solution: decision tree & summary table

<img src="../images/Data_layer_design_GCP_solution_decision_tree.png"
     alt="Data_layer_design_GCP_solution_decision_tree.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/Data_layer_design_GCP_solution_summary_table.png"
     alt="Data_layer_design_GCP_solution_summary_table.png"
     style="float: left; margin-right: 10px;" />


## Application: The photo service experiences Intermittent Outages

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/1NbrK/intermittent-outages)

There's something wrong with the photo application.

Occasionally, the service simply fails to produce the smaller preview image. This used to suddenly occur, but in recent weeks the frequency appears to be increasing and it is becoming a problem recognize by users. The issue appears to be **random**. 

- What's happening?
- and how can you change the design to fix this problem? 

### Business problem

<img src="../images/app_photo_service_intermittent_outages.png"
     alt="app_photo_service_intermittent_outages.png"
     style="float: left; margin-right: 10px;" />



### Systematic logical troubleshooting

<img src="../images/app_photo_service_intermittent_outages_analysis.png"
     alt="app_photo_service_intermittent_outages_analysis.png"
     style="float: left; margin-right: 10px;" />

- systematic & logical troubleshooting
- and answering the ["five why's"](https://en.wikipedia.org/wiki/Five_Whys)


The team has determined that the root problem, is the persistent disk on the application server cannot keep up with the scale of demands being placed on it. All right. So, that's the thing. You can scale up CPU, but sometimes your underlying disk IO performance cannot keep up. As well as something that most people don't think about, is disk IO also does need some CPU? So, that could be a problem. But in this case, we're focusing strictly on that. There's not enough virtual machine disc IO power to handle this kind of scale that we're looking at.

### Break down business logic on the photo service

<img src="../images/app_photo_service_refreshed_business_logic.png"
     alt="app_photo_service_refreshed_business_logic.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/app_photo_service_business_logic_with_bottleneck.png"
     alt="app_photo_service_business_logic_with_bottleneck.png"
     style="float: left; margin-right: 10px;" />

with 1 millions pictures per day, we need to move from our local file system to Cloud Storage for scalability.

### New Service Level Objectives (SLOs) and Indicators (SLIs)

we're going to introduce some **new service level objectives**.

In this case here, we want to **reduce the error rate**. So, we want to keep the failures to produce a thumbnail down below, less than one 10th of one percent. So, that's **about 100 errors per million images**.

Now, how are we going to measure this? While our service level indicators are going to look at the logs and we're going to identify if there's any logs that failed to, if they indicate any type of errors. So, this means that **our error budget is going to be 3,000 errors per month**. 

Now, one of the things that our IOs do, is they are allocated an error budget. As long as they stay within that budget, then they can work on future things. But if they're unable to remain within their error budget during the month, then they will have to focus on automation and tools, and whatever it takes, to ensure that they can now meet that error budget month after month before they can work on new things, or just simply take a break, right? Because that's really the goal, you don't want your SRE is working full time. They should only be working about half the time and if they are, it should be either responding to requests or developing for future uses. Take the job of a firefighter, they're always on-call, always on edge, but they're not physically working all the time. If you did that, then they wouldn't be able to offer that same kind of response.

<img src="../images/app_photo_service_outages_new_service_levels.png"
     alt="app_photo_service_outages_new_service_levels.png"
     style="float: left; margin-right: 10px;" />

## Design challenge #2: Complication

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/XVrs7/design-challenge-2-complication)

In the application lesson, the random intermittent failure turned out to be a **capacity issue**. The services simply couldn't keep up with the demand and you're able to redesign a solution to add a storage service component in addition to the web server component and the thumbnail processing app server component. 

This leads to **another problem in the log aggregation system**: The log aggregator needs to account for another source of log entries and **the log aggregation servers running out of disk space**. Take a closer look at this problem. 

Your challenge is to **modify the log aggregation design to avoid or overcome this issue**. Watch the lesson that describes the problem, then come up with your own solution. When you're ready, continue the lesson to see a sample solution. Remember that the sample solution is not the best possible solution. it's just an example and your design might 


<img src="../images/app_photo_service_capacity_problem_for_log_aggregation.png"
     alt="app_photo_service_capacity_problem_for_log_aggregation.png"
     style="float: left; margin-right: 10px;" />

**Business logic**

<img src="../images/app_photo_service_capacity_problem_for_log_aggregation_new_business_logic.png"
     alt="app_photo_service_capacity_problem_for_log_aggregation_new_business_logic.png"
     style="float: left; margin-right: 10px;" />

- Why not **GCS** for aggregating this log files?

<img src="../images/app_photo_service_capacity_problem_for_log_aggregation_properties.png"
     alt="app_photo_service_capacity_problem_for_log_aggregation_properties.png"
     style="float: left; margin-right: 10px;" />

- Why not **Bigtable** for aggregating this log files?

<img src="../images/app_photo_service_capacity_problem_for_log_aggregation_Bigtable.png"
     alt="app_photo_service_capacity_problem_for_log_aggregation_Bigtable.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/dummy.png"
     alt="dummy.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/dummy.png"
     alt="dummy.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/dummy.png"
     alt="dummy.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/dummy.png"
     alt="dummy.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/dummy.png"
     alt="dummy.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/dummy.png"
     alt="dummy.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/dummy.png"
     alt="dummy.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/dummy.png"
     alt="dummy.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/dummy.png"
     alt="dummy.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/dummy.png"
     alt="dummy.png"
     style="float: left; margin-right: 10px;" />




## Resources/Articles

- https://cloud.google.com/compute/docs/reference/rest/v1/instances