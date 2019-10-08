# Design for Resiliency, Scalability, Disaster Recovery & Security

- **Resiliency, Scalability, Disaster Recovery**: Implement technologies and processes that assure business continuity in the event of a disaster.
- **Security**: Implement policies that minimize security risks, such as auditing, separation of duties and least privilege.
- **Capacity Planning & Cost Optimization**: Identify ways to optimize resources and minimize cost.
- **Deployment, Monitoring and Alerting, and Incident Response**

## Content

* [Detailled Content](#detailled-content)
* [Design for Resiliency, Scalability, and Disaster Recovery](#design-for-resiliency-scalability-and-disaster-recovery)
   * [Overview](#overview)
   * [Failure Due to Loss](#failure-due-to-loss)
   * [Failure Due to Overload](#failure-due-to-overload)
   * [Coping with Failure](#coping-with-failure)
   * [Business Continuity &amp; Disaster Recovery](#business-continuity--disaster-recovery)
   * [Scalable &amp; Resilient Design](#scalable--resilient-design)
   * [Application: Out of Service!](#application-out-of-service)
   * [Design Challenge #4: Redesign for Time](#design-challenge-4-redesign-for-time)
* [Design for Security](#design-for-security)
   * [Overview](#overview-1)
   * [Cloud Security](#cloud-security)
   * [Network Access Control &amp; Firewalls](#network-access-control--firewalls)
   * [Protections Against Denial of Service](#protections-against-denial-of-service)
   * [Resource Sharing &amp; Isolation](#resource-sharing--isolation)
   * [Data Encryption &amp; Key Management](#data-encryption--key-management)
   * [Design for Security: Identity Access &amp; Auditing](#design-for-security-identity-access--auditing)
   * [Application: Photo Service - Intentional Attack](#application-photo-service---intentional-attack)
   * [Design Challenge #5: Defense in Depth](#design-challenge-5-defense-in-depth)
* [Capacity Planning &amp; Cost Optimization](#capacity-planning--cost-optimization)
   * [Overview](#overview-2)
   * [Capacity Planning](#capacity-planning)
   * [Pricing](#pricing)
   * [Application: Photo Service - Cost &amp; Capacity](#application-photo-service---cost--capacity)
   * [Design Challenge #6: Dimensioning](#design-challenge-6-dimensioning)
* [Deployment, Monitoring and Alerting, and Incident Response](#deployment-monitoring-and-alerting-and-incident-response)
   * [Learning Objectives](#learning-objectives)
   * [Overview](#overview-3)
   * [Deployment](#deployment)
   * [Monitoring &amp; Alerting](#monitoring--alerting)
   * [Incident Response](#incident-response)
   * [Application: Stabilization &amp; Operation](#application-stabilization--operation)
   * [Design Challenge #7: Monitoring &amp; Alerting](#design-challenge-7-monitoring--alerting)
   * [Lab: Deployment Manager - Full Production](#lab-deployment-manager---full-production)
* [Resources/Articles](#resourcesarticles)

## Detailled Content


* [Design for Resiliency, Scalability, and Disaster Recovery](#design-for-resiliency-scalability-and-disaster-recovery)
   * [Overview](#overview)
   * [Failure Due to Loss](#failure-due-to-loss)
   * [Failure Due to Overload](#failure-due-to-overload)
   * [Coping with Failure](#coping-with-failure)
   * [Business Continuity &amp; Disaster Recovery](#business-continuity--disaster-recovery)
   * [Scalable &amp; Resilient Design](#scalable--resilient-design)
   * [Application: Out of Service!](#application-out-of-service)
   * [Design Challenge #4: Redesign for Time](#design-challenge-4-redesign-for-time)
* [Design for Security](#design-for-security)
   * [Overview](#overview-1)
   * [Cloud Security](#cloud-security)
   * [Network Access Control &amp; Firewalls](#network-access-control--firewalls)
   * [Protections Against Denial of Service](#protections-against-denial-of-service)
   * [Resource Sharing &amp; Isolation](#resource-sharing--isolation)
   * [Data Encryption &amp; Key Management](#data-encryption--key-management)
   * [Design for Security: Identity Access &amp; Auditing](#design-for-security-identity-access--auditing)
   * [Application: Photo Service - Intentional Attack](#application-photo-service---intentional-attack)
   * [Design Challenge #5: Defense in Depth](#design-challenge-5-defense-in-depth)
* [Capacity Planning &amp; Cost Optimization](#capacity-planning--cost-optimization)
   * [Overview](#overview-2)
   * [Capacity Planning](#capacity-planning)
   * [Pricing](#pricing)
   * [Application: Photo Service - Cost &amp; Capacity](#application-photo-service---cost--capacity)
   * [Design Challenge #6: Dimensioning](#design-challenge-6-dimensioning)
* [Deployment, Monitoring and Alerting, and Incident Response](#deployment-monitoring-and-alerting-and-incident-response)
   * [Learning Objectives](#learning-objectives)
   * [Overview](#overview-3)
   * [Deployment](#deployment)
   * [Monitoring &amp; Alerting](#monitoring--alerting)
   * [Incident Response](#incident-response)
   * [Application: Stabilization &amp; Operation](#application-stabilization--operation)
   * [Design Challenge #7: Monitoring &amp; Alerting](#design-challenge-7-monitoring--alerting)
   * [Lab: Deployment Manager - Full Production](#lab-deployment-manager---full-production)
* [Resources/Articles](#resourcesarticles)


## Design for Resiliency, Scalability, and Disaster Recovery

Implement technologies and processes that assure business continuity in the event of a disaster.

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/pWlcV/design-for-resiliency-scalability-and-disaster-recovery-overview)

### Overview

This module deals with resiliency. The ability of a system to stay available and to bounce back from problems. But what is a resilient or available design? Is resiliency something you can add? Is it a feature you can turn on? Not really. Resiliency is the quality of a design that accounts for and handles failure. One principle of design is that sometimes a quality you want in the system isn't something you can really do anything about. To get the quality you want you have to look 180 degrees in the opposite direction. In this case, to get availability you have to look at and deal with the potential causes and sources of failure.

This module is designed for:

- **resiliency**,
- **scalability** and,
- **disaster recovery**.

<img src="../images/resiliency_definition.png"
     alt="resiliency_definition.png"
     style="float: left; margin-right: 10px;" />

we're gonna be covering failure; in general, failure due to a loss, failure due to overload. How do we cope with failure? Here, go through to a psychiatrist on that one. What about business continuity? How do we continue if there is a failure and disaster recovery? If there's something major, how do we recover completely from this? Then we will finally talk about scalable and resilient design, which is supposedly supposed to offset all of these bad things from happening or at least dealing with them at least. So then we're going to have an out-of-service issue with our photo service, and then we're going to have to redesign our logging system. 

### Failure Due to Loss

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/1RhEP/design-for-resiliency-scalability-and-disaster-recovery-failure-due-to-loss)

#### Failure is mandatory

For a period of time in the 1960s, a research team was trying to develop a perfect conductor. The theory was that if they could grow a perfect crystal and metal wire, there would be no signal loss and, therefore, no errors in communication. And they succeeded in creating the perfect wire. However, when they tested it, sometimes there were still errors. Do you know why? It's because we live in a quantum mechanical universe, and the location of electrons moving down a conductor is probabilistic. So, occasionally, an electron will appear outside of the wire and get lost. Errors are going to happen. Loss is going to happen.

> So the challenge isn't to avoid it, but to accept it and deal with it.

In this lesson, you'll learn about designing systems to handle failure caused by loss of resources.

<img src="../images/resiliency_failure_is_mandatory.png"
     alt="resiliency_failure_is_mandatory.png"
     style="float: left; margin-right: 10px;" />

#### Single Point of failure

<img src="../images/resiliency_failure_single_point_of_failure.png"
     alt="resiliency_failure_single_point_of_failure.png"
     style="float: left; margin-right: 10px;" />

#### Design to avoid Single Point of failure: N+2 (a spare spare)

<img src="../images/resiliency_failure_design_for_single_point_of_failure.png"
     alt="resiliency_failure_design_for_single_point_of_failure.png"
     style="float: left; margin-right: 10px;" />

#### Correlated failures

<img src="../images/resiliency_failure_correlated_failures.png"
     alt="resiliency_failure_correlated_failures.png"
     style="float: left; margin-right: 10px;" />

#### Design to avoid correlated failures

<img src="../images/resiliency_failure_design_to_avoid_correlated_failures.png"
     alt="resiliency_failure_design_to_avoid_correlated_failures.png"
     style="float: left; margin-right: 10px;" />


### Failure Due to Overload

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/LbKLf/design-for-resiliency-scalability-and-disaster-recovery-failure-due-to-overload)

When a system is overloaded at some point the system crosses over into nonlinear behavior. It can crash, thrash, stop responding or break adjacent resources at the service depends on. There's a very specific relationship between failure due to loss and failure due to overload. Imagine for example that a resource is lost and you queue up the work for that resource. All the work that comes in gets backlogged. When the resource is restored, there's this tremendous backlog of work to get done before any new work can be handled and if there's enough of it, nothing new ever gets done. So then the system goes from being totally unavailable due to resource loss, to totally unavailable due to overload. In this lesson you'll learn about the common causes of overload failure and how to plan for them and deal with them. When it comes to overload, prevention is really the best solution so design is critical.

<img src="../images/resiliency_failure_due_to_overload.png"
     alt="resiliency_failure_due_to_overload.png"
     style="float: left; margin-right: 10px;" />

#### Failover design for reliability

<img src="../images/resiliency_failure_due_to_overload_failover_design_for_reliability.png"
     alt="resiliency_failure_due_to_overload_failover_design_for_reliability.png"
     style="float: left; margin-right: 10px;" />

#### Cascading failures

<img src="../images/resiliency_failure_due_to_overload_cascading_failures.png"
     alt="resiliency_failure_due_to_overload_cascading_failures.png"
     style="float: left; margin-right: 10px;" />

#### Design to avoid Cascading failures

<img src="../images/resiliency_failure_due_to_overload_design_to_avoid_cascading_failures.png"
     alt="resiliency_failure_due_to_overload_design_to_avoid_cascading_failures.png"
     style="float: left; margin-right: 10px;" />

Example:

<img src="../images/resiliency_failure_due_to_overload_design_to_avoid_cascading_failures_example.png"
     alt="resiliency_failure_due_to_overload_design_to_avoid_cascading_failures_example.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/resiliency_failure_due_to_overload_design_to_avoid_cascading_failures_mitigate_incast_failure.png"
     alt="resiliency_failure_due_to_overload_design_to_avoid_cascading_failures_mitigate_incast_failure.png"
     style="float: left; margin-right: 10px;" />

#### Queries-of-Death overload failure

<img src="../images/resiliency_failure_due_to_overload_QueriesOfDeath_failures.png"
     alt="resiliency_failure_due_to_overload_QueriesOfDeath_failures.png"
     style="float: left; margin-right: 10px;" />

#### Positive feedback cycle overload failure

<img src="../images/resiliency_failure_due_to_overload_positive_feedback_overload_failures.png"
     alt="resiliency_failure_due_to_overload_positive_feedback_overload_failures.png"
     style="float: left; margin-right: 10px;" />

#### Detect overload early: Early warning systems (_canaries_)

<img src="../images/resiliency_failure_due_to_overload_detect_early_canaries.png"
     alt="resiliency_failure_due_to_overload_detect_early_canaries.png"
     style="float: left; margin-right: 10px;" />

### Coping with Failure

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/jm9LV/design-for-resiliency-scalability-and-disaster-recovery-coping-with-failure)


The time to prepare for an emergency is before it happens. Moreover, the way to prepare for failure is to embrace it, except that sooner or later, it's inevitable. By making the processes and behaviors you want part of the normal routine operations, you avoid the surprise.

For example, if you know that a zone outage is possible, consider establishing rotating outages as part of the routine operations. Here's another idea, if you know that the users expect 99.95 percent availability, and your service is operating at 99.99 percent availability, consider using that 0.04 percent gap to exercise your resiliency and recovery designs. Finally, don't underestimate the importance of meetings. Circumstances are going to change, if you surround the technical processes with the right human processes, the team will catch the issues before they become emergencies.

<img src="../images/resiliency_coping_with_failure.png"
     alt="resiliency_coping_with_failure.png"
     style="float: left; margin-right: 10px;" />


#### Forest fires or Controlled burns

<img src="../images/resiliency_coping_with_failure_forest_fire_or_controlled_burn.png"
     alt="resiliency_coping_with_failure_forest_fire_or_controlled_burn.png"
     style="float: left; margin-right: 10px;" />

#### Prepare the team

<img src="../images/resiliency_coping_with_failure_prepare_the_team.png"
     alt="resiliency_coping_with_failure_prepare_the_team.png"
     style="float: left; margin-right: 10px;" />

#### Incorporate failure into SLOs

<img src="../images/resiliency_coping_with_failure_include_in_SLOs.png"
     alt="resiliency_coping_with_failure_include_in_SLOs.png"
     style="float: left; margin-right: 10px;" />

#### Monthly meetings to build processes

<img src="../images/resiliency_coping_with_failure_meeting_monthly_meetings.png"
     alt="resiliency_coping_with_failure_meeting_monthly_meetings.png"
     style="float: left; margin-right: 10px;" />

#### Strategies for dealing with failure

<img src="../images/resiliency_coping_with_failure_strategies.png"
     alt="resiliency_coping_with_failure_strategies.png"
     style="float: left; margin-right: 10px;" />


### Business Continuity & Disaster Recovery

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/FYikQ/design-for-resiliency-scalability-and-disaster-recovery-business-continuity-and)

The overall strategy for business continuity can be summed up in this motto;

> No surprises.

Whatever's happening or could happen, you want to find out about it early and you want to give yourself plenty of recovery options in the design of your system. Now that's a balance.

How much resource and energy do you want to spend on insurance? You might have great recovery systems built into your design, but how do you know they're working, and how do you know that something hasn't changed and those systems haven't quietly stopped working? You need to understand what level of testing and exercise of the recovery systems gives you confidence. That will help you decide what to include in the design elements and also help shape the human processes that operate and maintain the system. 

<img src="../images/resiliency_business_continuity_disaster_recovery.png"
     alt="resiliency_business_continuity_disaster_recovery.png"
     style="float: left; margin-right: 10px;" />

#### Cloud DNS: 100% availability

<img src="../images/resiliency_business_continuity_disaster_recovery_CloudDNS.png"
     alt="resiliency_business_continuity_disaster_recovery_CloudDNS.png"
     style="float: left; margin-right: 10px;" />

#### Data Integrity

<img src="../images/resiliency_business_continuity_disaster_recovery_data_integrity.png"
     alt="resiliency_business_continuity_disaster_recovery_data_integrity.png"
     style="float: left; margin-right: 10px;" />

#### Reliable recovery with Lazy Deletion

<img src="../images/resiliency_business_continuity_disaster_recovery_lazy_or_soft_deletion.png"
     alt="resiliency_business_continuity_disaster_recovery_lazy_or_soft_deletion.png"
     style="float: left; margin-right: 10px;" />

#### backup, archive, RESTORE!

<img src="../images/resiliency_business_continuity_disaster_recovery_backup_archive_restore.png"
     alt="resiliency_business_continuity_disaster_recovery_backup_archive_restore.png"
     style="float: left; margin-right: 10px;" />

#### Tiered backup for resiliency

<img src="../images/resiliency_business_continuity_disaster_tiered_backup_services.png"
     alt="resiliency_business_continuity_disaster_tiered_backup_services.png"
     style="float: left; margin-right: 10px;" />

##### Cloud Storage features for backup and DR

<img src="../images/resiliency_business_continuity_disaster_tiered_backup_services_cloud_Storage.png"
     alt="resiliency_business_continuity_disaster_tiered_backup_services_cloud_Storage.png"
     style="float: left; margin-right: 10px;" />

#### Prepare the team for disasters

<img src="../images/resiliency_business_continuity_disaster_practice_document_prepare_the_team.png"
     alt="resiliency_business_continuity_disaster_practice_document_prepare_the_team.png"
     style="float: left; margin-right: 10px;" />


### Scalable & Resilient Design

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/E9vLK/design-for-resiliency-scalability-and-disaster-recovery-scalable-and-resilient) 

Vertical scaling makes components bigger, but it leaves them as a single point of failure. For example, swapping out a single VM for VM with larger capacity, still means that VM can fail and potentially crash your service. Horizontal scaling make services bigger through multiplicity. It leaves a unit capacity the same, but increases the pool of units. For example, instead of growing a larger VM, you could move to a design with a pool consisting of multiple smaller VMs. That not only makes it scalable, but resilient, because if one VM is lost, the others can pick up the workload until replacement is added. There are several steps you can take to make your design resilient, and they're covered in this lesson

1. **Health checks to monitor instances**

<img src="../images/resiliency_design_health_checks.png"
     alt="resiliency_design_health_checks.png"
     style="float: left; margin-right: 10px;" />

2. **Automatically replace instances**

<img src="../images/resiliency_design_auto_replace_instances.png"
     alt="resiliency_design_auto_replace_instances.png"
     style="float: left; margin-right: 10px;" />

3. Resilient Storage: Cloud Storage, Cloud SQL

<img src="../images/resiliency_design_resilient_storage.png"
     alt="resiliency_design_resilient_storage.png"
     style="float: left; margin-right: 10px;" />

4. Resilient Network

<img src="../images/resiliency_design_resilient_network.png"
     alt="resiliency_design_resilient_network.png"
     style="float: left; margin-right: 10px;" />

#### Design pattern: General design for scalable & resilient apps

<img src="../images/resiliency_design_design_pattern_general_design_for_scalable_resilient_apps.png"
     alt="resiliency_design_design_pattern_general_design_for_scalable_resilient_apps.png"
     style="float: left; margin-right: 10px;" />


* Handles **loss of instance**

<img src="../images/general_design_for_scalable_resilient_apps_loss_of_instance.png"
     alt="general_design_for_scalable_resilient_apps_loss_of_instance.png"
     style="float: left; margin-right: 10px;" />

* Handles **loss of zone**

<img src="../images/general_design_for_scalable_resilient_apps_loss_of_zone.png"
     alt="general_design_for_scalable_resilient_apps_loss_of_zone.png"
     style="float: left; margin-right: 10px;" />

* Handles **loss of database**

<img src="../images/general_design_for_scalable_resilient_apps_loss_of_database.png"
     alt="general_design_for_scalable_resilient_apps_loss_of_database.png"
     style="float: left; margin-right: 10px;" />

* Handles **full disaster recovery**

<img src="../images/general_design_for_scalable_resilient_apps_full_disaster_recovery.png"
     alt="general_design_for_scalable_resilient_apps_full_disaster_recovery.png"
     style="float: left; margin-right: 10px;" />

#### Microservices design for scalable & resilient streaming

<img src="../images/resiliency_design_design_pattern_microservices_design.png"
     alt="resiliency_design_design_pattern_microservices_design.png"
     style="float: left; margin-right: 10px;" />

#### 12-factor system & application design in GCP

<img src="../images/resiliency_design_design_pattern_12-factor_design.png"
     alt="resiliency_design_design_pattern_12-factor_design.png"
     style="float: left; margin-right: 10px;" />

#### Processes for simple, iterative, aligned development

<img src="../images/resiliency_design_processes.png"
     alt="resiliency_design_processes.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/resiliency_design_processes_iterate.png"
     alt="resiliency_design_processes_iterate.png"
     style="float: left; margin-right: 10px;" />

This module covered several related design goals, including:

* reliability,
* scalability,
* and disaster recovery.

The first subject was availability and reliability. A key concept is that planning for failure and dealing with failure in your design leads to improved reliability. Failure can occur due to the loss of a resource or it can occur due to overload. You must be careful when making adjustments to a system, that you don't accidentally create the potential for an overload failure when you're trying to improve resiliency to a loss failure. The second subject was disaster recovery. You learned that planning for disaster and preparing for recovery is key. The third was scalable and resilient design. That brings together many of the design principles you've seen in the previous modules, and shows how they all fit together into a general resilient solution.


### Application: Out of Service!

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/SeWZH/out-of-service) 

#### Business problem

It's a **Design problem**, we lost an entire zone!!!

<img src="../images/application_photo_Service_crashed.png"
     alt="application_photo_Service_crashed.png"
     style="float: left; margin-right: 10px;" />

The popular and growing photo services suddenly crashed:

* How do you handle a major outage?
* What could the cause be? What changes can you make the design so this problem doesn't take down the entire service again in the future?

<img src="../images/application_photo_Service_crashed_problem.png"
     alt="application_photo_Service_crashed_problem.png"
     style="float: left; margin-right: 10px;" />


**Have a plan for dealing with a major outage**

<img src="../images/application_photo_Service_crashed_solution_have_process_in_place.png"
     alt="application_photo_Service_crashed_solution_have_process_in_place.png"
     style="float: left; margin-right: 10px;" />


#### Systematic logical troubleshooting

**Service loss due to zone outage**

<img src="../images/application_photo_Service_crashed_troubleshooting.png"
     alt="application_photo_Service_crashed_troubleshooting.png"
     style="float: left; margin-right: 10px;" />


#### Collaboration & communication: Report, Document, build policy

So you want to **move these servers to multiple zones.**

<img src="../images/application_photo_Service_crashed_solution_multiple_zones.png"
     alt="application_photo_Service_crashed_solution_multiple_zones.png"
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


#### Break down business logic on the photo service

#### What about our Service Level Objectives (SLOs) and Indicators (SLIs)

SLOs didn't change again. We just added more redundancy, making the service span multiple zones.


<img src="../images/application_photo_Service_crashed_SLOs.png"
     alt="application_photo_Service_crashed_SLOs.png"
     style="float: left; margin-right: 10px;" />


### Design Challenge #4: Redesign for Time



<img src="../images/application_photo_Service_crashed_again_SinglePointOfFAilure.png"
     alt="application_photo_Service_crashed_again_SinglePointOfFAilure.png"
     style="float: left; margin-right: 10px;" />


**Need to scale frontend server**

<img src="../images/application_photo_Service_crashed_again_solution_scale_upload_servers_Across_zones.png"
     alt="application_photo_Service_crashed_again_solution_scale_upload_servers_Across_zones.png"
     style="float: left; margin-right: 10px;" />

**Prevent overload: MAke load testing real**

<img src="../images/application_photo_Service_crashed_again_prevent_overload_with_real_testing.png"
     alt="application_photo_Service_crashed_again_prevent_overload_with_real_testing.png"
     style="float: left; margin-right: 10px;" />

**Scaling requires breaking state out of the upload server**

<img src="../images/application_photo_Service_crashed_again_scale_upload_server_stateless.png"
     alt="application_photo_Service_crashed_again_scale_upload_server_stateless.png"
     style="float: left; margin-right: 10px;" />


What does that look like?

**Need to make Frontend servers stateless**

<img src="../images/application_photo_Service_crashed_again_scale_upload_server_stateless_how.png"
     alt="application_photo_Service_crashed_again_scale_upload_server_stateless_how.png"
     style="float: left; margin-right: 10px;" />

**Update on SLOs and SLIs**

You see what it is that you can measure and then quantify it.

<img src="../images/application_photo_Service_crashed_again_new_SLOs.png"
     alt="application_photo_Service_crashed_again_new_SLOs.png"
     style="float: left; margin-right: 10px;" />


#### Design Challenge: Log aggregation delayed troubleshooting

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/9EsdO/design-challenge-4-redesign-for-time)

When the photo service crashed, it became evident that troubleshooting was taking too long. The reason for the delay was tracked to log aggregation. The aggregated logs needed for troubleshooting were delayed. Worse, the more problems that are occurring in the system, the more log entries are generated and the longer it takes for the aggregated logs to become available for troubleshooting. Can you redesign the log system to eliminate the bottlenecks? Watch the lesson that describes the problem, then come up with your own solution. When you're ready, continue the lesson to see a sample solution.

<img src="../images/design_challenge_redesign_for_time.png"
     alt="design_challenge_redesign_for_time.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/design_challenge_redesign_for_time_problem.png"
     alt="design_challenge_redesign_for_time_problem.png"
     style="float: left; margin-right: 10px;" />

If you remember the **12-factor design**, it says to **treat log events as streams** so that should be a clue. The business issue is servant's service resiliency. It's just taking too long to troubleshoot service issues, batch processing of the logs, just simply does not support live service troubleshooting. It's causing delays, we can't meet our service level objectives, we can't identify and respond to incidents in times. So here's the design challenge. Replace the cron batch processing with stream processing and here's our hint. Try to consider a microservices design.


<img src="../images/design_challenge_redesign_for_time_challenge.png"
     alt="design_challenge_redesign_for_time_challenge.png"
     style="float: left; margin-right: 10px;" />

#### Troubleshooting & solution

<img src="../images/design_challenge_redesign_for_time_troubleshooting.png"
     alt="design_challenge_redesign_for_time_troubleshooting.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/design_challenge_redesign_for_time_solution.png"
     alt="design_challenge_redesign_for_time_solution.png"
     style="float: left; margin-right: 10px;" />

Instead of having a cronjob, we have not converted it to Stream Processing with Cloud Pub/Sub.





## Design for Security

Implement policies that minimize security risks, such as auditing, separation of duties and least privilege.

### Overview

### Cloud Security

### Network Access Control & Firewalls

### Protections Against Denial of Service

### Resource Sharing & Isolation

### Data Encryption & Key Management

### Design for Security: Identity Access & Auditing

### Application: Photo Service - Intentional Attack

### Design Challenge #5: Defense in Depth



<img src="../images/dummy.png"
     alt="dummy.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/dummy.png"
     alt="dummy.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/dummy.png"
     alt="dummy.png"
     style="float: left; margin-right: 10px;" />




## Capacity Planning & Cost Optimization

Identify ways to optimize resources and minimize cost.

### Overview

### Capacity Planning

### Pricing

### Application: Photo Service - Cost & Capacity

### Design Challenge #6: Dimensioning



## Deployment, Monitoring and Alerting, and Incident Response

### Learning Objectives

* Implement processes that minimize downtime, such as monitoring and alarming, unit and integration testing, production resilience testing, and incident post-mortem analysis.
* Launch a cloud service from a collection of templates.
* Configure basic black box monitoring of an application.
* Create an uptime check to recognize a loss of service.
* Establish an alerting policy to trigger incident response procedures.
* Create and configure a dashboard with dynamically update charts.
* Test the monitoring and alerting regimen by applying a load to the service.
* Test the monitoring and alerting regimen by simulating a service outage.

### Overview

### Deployment

### Monitoring & Alerting

### Incident Response

### Application: Stabilization & Operation

### Design Challenge #7: Monitoring & Alerting

### Lab: Deployment Manager - Full Production

## Resources/Articles