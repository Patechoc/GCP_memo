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

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/XjMe5/lab-intro-deployment-manager)

- understand YAML templates syntax
- deploy a single appserver with Deployment Manager

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

