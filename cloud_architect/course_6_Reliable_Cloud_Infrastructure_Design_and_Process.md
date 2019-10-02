# Reliable Cloud Infrastructure: Design and Process

## Content

ToC


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


## 4 parts of this class

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

