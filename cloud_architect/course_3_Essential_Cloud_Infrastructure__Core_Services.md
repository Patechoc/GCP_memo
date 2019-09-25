# Essential Cloud Infrastructure: Core Services


The goal of these courses is:

* to remember and understand the **different GCP services** and **features** and also
* to be able to apply your knowledge, 
* to analyze requirements,
* to evaluate different options, and 
* to create your own services.

This course builds on the [Essential Cloud Infrastructure: Foundation](./cloud_architect/course_2_Essential_Cloud_Infrastructure__Foundation.md) course and enhances your study of architecting with Compute Engine. In this course, we start by talking about Cloud IAM, and you will administer Identity and Access Management for resources. Next, we’ll cover the different data storage services in GCP, and you will implement some of those services. Then, we’ll go over resource management, where you will manage and examine billing of GCP resources. Lastly, we’ll talk about resource monitoring, and you will monitor GCP resources using Stackdriver services. 


> "We want to welcome you to Architecting with Compute Engine".

## Content

- [course intro video](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/6Zi73/course-introduction)
- [course review video on storage/database services](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/EismW/module-review)


* [Cloud Identity and Access Management (Cloud IAM)](#cloud-identity-and-access-management-cloud-iam)
    * [Organizations](#organizations)
    * [Folders: sub-organizations](#folders-sub-organizations)
    * [Roles](#roles)
    * [Demo: Create a Custom role in GCP](#demo-create-a-custom-role-in-gcp)
    * [Members](#members)
    * [Service Accounts](#service-accounts)
    * [Scopes](#scopes)
    * [Cloud IAM best practices](#cloud-iam-best-practices)
    * [Cloud IAP](#cloud-iap)
    * [Lab Intro: Cloud IAM](#lab-intro-cloud-iam)
* [Data Storage Services](#data-storage-services)
    * [Cloud Storage](#cloud-storage)
    * [Cloud Storage lab](#cloud-storage-lab)
    * [Cloud SQL](#cloud-sql)
    * [Cloud SQL lab](#cloud-sql-lab)
    * [Cloud Spanner](#cloud-spanner)
    * [Cloud Firestore](#cloud-firestore)
    * [Cloud Bigtable](#cloud-bigtable)
    * [Cloud Memorystore (Redis)](#cloud-memorystore-redis)
* [Resource Management](#resource-management)
    * [Cloud Resource](#cloud-resource)
    * [Quotas](#quotas)
    * [Labels and names](#labels-and-names)
    * [Billing](#billing)
    * [Use Labels to optimize spendings](#use-labels-to-optimize-spendings)
    * [Lab: Billing Administration](#lab-billing-administration)
* [Resource Monitoring](#resource-monitoring)
* [Resources/Articles](#resourcesarticles)


-  more than one solution for a task or application in GCP, a [solution continuum](https://github.com/Patechoc/GCP_memo/blob/master/cloud_architect/course_2_Essential_Cloud_Infrastructure__Foundation.md#gcp-infrastruture)
- Infrastructure, Users, Applications [analogy](https://github.com/Patechoc/GCP_memo/blob/master/cloud_architect/course_2_Essential_Cloud_Infrastructure__Foundation.md#gcp-infrastruture)

GCP offers a **range of Compute Services**

<img src="../images/core_services_Compute_Services.png"
        alt="core_services_Compute_Services.png"
        style="float: left; margin-right: 10px;" />


## Cloud Identity and Access Management (Cloud IAM)

- [Cloud IAM overview](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/UbUc3/cloud-iam)

Cloud IAM is a sophisticated system built on top of.

- email-like address names,
- job type roles in granular permissions.

Components within Cloud IAM which are **organizations**, **roles**, **members** and **service accounts**


[video](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/UbUc3/cloud-iam) 

Who can do What on Which resources:

<img src="../images/core_services_Cloud_IAM_Who_DoWhat_WhichResource.png"
        alt="core_services_Cloud_IAM_Who_DoWhat_WhichResource.png"
        style="float: left; margin-right: 10px;" />


Cloud IAM objects: 

<img src="../images/core_services_Cloud_IAM_objects.png"
        alt="core_services_Cloud_IAM_objects.png"
        style="float: left; margin-right: 10px;" />


Cloud IAM resource hierarchy:

<img src="../images/core_services_Cloud_IAM_resource_hierarchy.png"
        alt="core_services_Cloud_IAM_resource_hierarchy.png"
        style="float: left; margin-right: 10px;" />

> Apply the **Principle of Least Priviledges**: Always select the smallest scope that's necessary for the task in order to reduce your exposure to risk!

### Organizations

[video](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/M0m2G/organization)

- **"Organization Admin"**
- **"Project Creator"**

<img src="../images/core_services_Cloud_IAM_resource_Organization.png"
        alt="core_services_Cloud_IAM_resource_Organization.png"
        style="float: left; margin-right: 10px;" />

Responsabilities of:

- **"Cloud Identity account manager"**
- **"Organization Admin"**

<img src="../images/core_services_GCP_resources.png"
        alt="core_services_GCP_resources.png"
        style="float: left; margin-right: 10px;" />


### Folders: sub-organizations

Folders can act as a sub-organization wihtin the organization.

<img src="../images/core_services_Cloud_IAM_resource_Folders.png"
        alt="core_services_Cloud_IAM_resource_Folders.png"
        style="float: left; margin-right: 10px;" />

<img src="../images/core_services_Cloud_IAM_resource_every_Roles_of_a_project.png"
        alt="core_services_Cloud_IAM_resource_every_Roles_of_a_project.png"
        style="float: left; margin-right: 10px;" />


### Roles

[video](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/rgNsR/roles)

= Roles define the can do what on which resource part of Cloud IAM


There are three types of roles in Cloud IAM; **Primitive roles**, **predefined roles**, and **custom roles**:

<img src="../images/core_services_Cloud_IAM_Roles.png"
        alt="core_services_Cloud_IAM_Roles.png"
        style="float: left; margin-right: 10px;" />

- **Primitive roles** are the original roles that were available in the GCP console, but they are broad. You apply them to a GCP project and they affect all resources in that project.

<img src="../images/core_services_Cloud_IAM_Roles_primitive.png"
        alt="core_services_Cloud_IAM_Roles_primitive.png"
        style="float: left; margin-right: 10px;" />

<img src="../images/core_services_Cloud_IAM_Roles_primitive_coarse_grained.png"
        alt="core_services_Cloud_IAM_Roles_primitive_coarse_grained.png"
        style="float: left; margin-right: 10px;" />

- **Predefined roles** define where those roles can be applied. This provides members with granular access to specific GCP resources and prevents unwanted access to other resources. These roles are collections of permissions because to do any meaningful operations, you usually need more than one permission.

<img src="../images/core_services_Cloud_IAM_Roles_predefined.png"
        alt="core_services_Cloud_IAM_Roles_predefined.png"
        style="float: left; margin-right: 10px;" />


<img src="../images/core_services_Cloud_IAM_Roles_predefined_ex.png"
        alt="core_services_Cloud_IAM_Roles_predefined_ex.png"
        style="float: left; margin-right: 10px;" />


<img src="../images/core_services_Cloud_IAM_Roles_predefined_all.png"
        alt="core_services_Cloud_IAM_Roles_predefined_all.png"
        style="float: left; margin-right: 10px;" />

But what if one of those roles does not have enough permissions or you need something even finer-grained?

That's what **Custom Roles** permit: a lot of companies use the **least privileged model** in which each person in your organization is given the minimal amount of privilege needed to do their job.

<img src="../images/core_services_Cloud_IAM_Roles_custom.png"
        alt="core_services_Cloud_IAM_Roles_custom.png"
        style="float: left; margin-right: 10px;" />

#### Demo: Create a Custom role in GCP

[video](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/3bQES/demo-custom-roles)



### Members

[video](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/4vrs0/members)

= **Members** define the who part, of who can do what, on which resource.

<img src="../images/core_services_Cloud_IAM_Members.png"
        alt="core_services_Cloud_IAM_Members.png"
        style="float: left; margin-right: 10px;" />




There are 5 different types of members:

- **Google Accounts**: a Google Account represents a developer, an administrator or any other person who interacts with GCP
- **Service Accounts**,
- **Google Groups**,
- **G Suite Domains**, and
- **Cloud Identity Domains**. 


 What if you already have a different corporate directory? How can you get your users and groups into GCP? 
 
Use **Google Cloud Directory Sync**!!

<img src="../images/core_services_Cloud_IAM_remote_LDAP_Members.png"
        alt="core_services_Cloud_IAM_remote_LDAP_Members.png"
        style="float: left; margin-right: 10px;" />


Many ways to sync your remote account:

<img src="../images/core_services_Cloud_IAM_remote_LDAP_Members_SSO.png"
        alt="core_services_Cloud_IAM_remote_LDAP_Members_SSO.png"
        style="float: left; margin-right: 10px;" />




### Service Accounts

[video](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/292ex/service-accounts)


 A service account is an account that belongs to your application instead of to an individual end user

<img src="../images/core_services_Cloud_IAM_service_account_id.png"
        alt="core_services_Cloud_IAM_service_account_id.png"
        style="float: left; margin-right: 10px;" />

<img src="../images/core_services_Cloud_IAM_service_account_default_for_app_engine.png"
        alt="core_services_Cloud_IAM_service_account_default_for_app_engine.png"
        style="float: left; margin-right: 10px;" />

 
#### Scopes

<img src="../images/core_services_Cloud_IAM_service_account_scope.png"
        alt="core_services_Cloud_IAM_service_account_scope.png"
        style="float: left; margin-right: 10px;" />


<img src="../images/core_services_Cloud_IAM_user_created_service_accounts_only_predefined_roles.png"
        alt="core_services_Cloud_IAM_user_created_service_accounts_only_predefined_roles.png"
        style="float: left; margin-right: 10px;" />


Services Accounts use keys:

<img src="../images/core_services_Cloud_IAM_service_accounts_use_keys.png"
        alt="core_services_Cloud_IAM_service_accounts_use_keys.png"
        style="float: left; margin-right: 10px;" />

### Cloud IAM best practices

- [Module intro video](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/xghL2/cloud-iam-best-practices)
- [Module Review video](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/u8ETv/module-review)

<img src="../images/core_services_Cloud_IAM_best_practices_resource_hierarchy.png"
        alt="core_services_Cloud_IAM_best_practices_resource_hierarchy.png"
        style="float: left; margin-right: 10px;" />

Grant roles to groups instead of individuals:

<img src="../images/core_services_Cloud_IAM_best_practices_groups.png"
        alt="core_services_Cloud_IAM_best_practices_groups.png"
        style="float: left; margin-right: 10px;" />


Example of best practices when using Service Accounts:

<img src="../images/core_services_Cloud_IAM_best_practices_example_service_Accounts.png"
        alt="core_services_Cloud_IAM_best_practices_example_service_Accounts.png"
        style="float: left; margin-right: 10px;" />

#### Cloud IAP


Cloud IAP lets you establish a central authorization layer for applications accessed by HTTPS.
So you can use an application level access control model instead of relying on network level firewalls. Applications and resources protected by Cloud IAP can only be accessed through the proxy by users and groups with the correct Cloud IAM role. 


<img src="../images/core_services_Cloud_IAM_best_practices_Cloud_IAP.png"
        alt="core_services_Cloud_IAM_best_practices_Cloud_IAP.png"
        style="float: left; margin-right: 10px;" />

### Lab Intro: Cloud IAM

- [lab notes](../labs/lab_Cloud_IAM.md)
- [walkthrough video](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/mpXBr/lab-review-cloud-iam)


## Data Storage Services

- [Module intro video](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/HBW4r/module-overview)
- Module Review video


The purpose of this module is to explain which services are available and when to consider using them from an **infrastructure perspective**.

> "I want you to be able to set up and connect to a service without detailed knowledge of how to use a database system. If you want a deeper dive into the design, organizations, structures, schemas, and details on how data can be optimized,served, and stored properly within those different services, I recommend **Google Cloud's data engineering courses**"

<img src="../images/core_services_Data_Storage_module_scope.png"
        alt="core_services_Data_Storage_module_scope.png"
        style="float: left; margin-right: 10px;" />


Google offers several storage services to choose from:

- Cloud Storage
- Cloud SQL
- Cloud Spanner
- Cloud Firestore
- and Cloud Bigtable.

<img src="../images/core_services_Data_Storage_services.png"
        alt="core_services_Data_Storage_services.png"
        style="float: left; margin-right: 10px;" />


Choose using this decision tree:

<img src="../images/core_services_Data_Storage_services_decision_tree.png"
        alt="core_services_Data_Storage_services_decision_tree.png"
        style="float: left; margin-right: 10px;" />

### Cloud Storage

[video](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/MXyDu/cloud-storage)

Overview of storage classes:

- Regional
- Multi-regional
- Nearline
- Coldline

<img src="../images/core_services_Data_Storage_storage_classes.png"
        alt="core_services_Data_Storage_storage_classes.png"
        style="float: left; margin-right: 10px;" />


Cloud Storage entities:

- buckets
- objects
- access

<img src="../images/core_services_Data_Storage_storage_entities.png"
        alt="core_services_Data_Storage_storage_entities.png"
        style="float: left; margin-right: 10px;" />


Changing default storage class:

<img src="../images/core_services_Data_Storage_storage_changing_classes.png"
        alt="core_services_Data_Storage_storage_changing_classes.png"
        style="float: left; margin-right: 10px;" />

<img src="../images/core_services_Data_Storage_storage_access_control.png"
        alt="core_services_Data_Storage_storage_access_control.png"
        style="float: left; margin-right: 10px;" />

Zooming on ACL (Access Control Lists) = Who can access your bucket and what they can do (100 ACLs max). 

<img src="../images/core_services_Data_Storage_storage_access_control_ACLs.png"
        alt="core_services_Data_Storage_storage_access_control_ACLs.png"
        style="float: left; margin-right: 10px;" />


Limited-time access to a user:

`gsutil signurl -d 10m path/to/privatekey.p12 gs://bucket/object`

<img src="../images/core_services_Data_Storage_storage_limited_time_Access_to_user.png"
        alt="core_services_Data_Storage_storage_limited_time_Access_to_user.png"
        style="float: left; margin-right: 10px;" />

Cloud Storage Features

([video](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/wKUHL/cloud-storage-features))

<img src="../images/core_services_Data_Storage_storage_features.png"
        alt="core_services_Data_Storage_storage_features.png"
        style="float: left; margin-right: 10px;" />


Object versioning (OFF by default)

<img src="../images/core_services_Data_Storage_storage_features_object_versioning.png"
        alt="core_services_Data_Storage_storage_features_object_versioning.png"
        style="float: left; margin-right: 10px;" />

Object lifecycle management (rule-based actions: e.g. delete an object after a year, ...)

<img src="../images/core_services_Data_Storage_storage_features_object_lifecycle_management.png"
        alt="core_services_Data_Storage_storage_features_object_lifecycle_management.png"
        style="float: left; margin-right: 10px;" />

Object Notification with webhooks

<img src="../images/core_services_Data_Storage_storage_features_object_change_notifications.png"
        alt="core_services_Data_Storage_storage_features_object_change_notifications.png"
        style="float: left; margin-right: 10px;" />

Data Import services (TeraBytes of data):

- **Transfer Appliance** (hardware appliance from 100TB to 1 PetaBytes)
- **Storage Transfer Service** (from online data, e.g. from AWS bucket)
- **Offline Media Import**: 3rd party provider uploads data from physical media 

<img src="../images/core_services_Data_Storage_storage_features_data_import_services.png"
        alt="core_services_Data_Storage_storage_features_data_import_services.png"
        style="float: left; margin-right: 10px;" />

Decision tree to choose your storage class

<img src="../images/core_services_Data_Storage_storage_decision_tree_class_storage.png"
        alt="core_services_Data_Storage_storage_decision_tree_class_storage.png"
        style="float: left; margin-right: 10px;" />


### Cloud Storage lab

- [lab tasks description](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/bcncM/lab-intro-cloud-storage)
- [lab notes](../labs/lab_Cloud_Storage.md)
- [Lab Review: Cloud Storage](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/6IAAh/lab-review-cloud-storage)

### Cloud SQL

- [module intro video](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/cMja1/cloud-sql)

Cloud SQL is a **"managed service"** offering an _SQL database_ **service**.

<img src="../images/core_services_cloudSQL_manage_service.png"
        alt="core_services_cloudSQL_manage_service.png"
        style="float: left; margin-right: 10px;" />

Performances
<img src="../images/core_services_cloudSQL_performance.png"
        alt="core_services_cloudSQL_performance.png"
        style="float: left; margin-right: 10px;" />

Optional features (backup, automatic failover, import/export and scaling)

<img src="../images/core_services_cloudSQL_optional_features.png"
        alt="core_services_cloudSQL_optional_features.png"
        style="float: left; margin-right: 10px;" />

Connecting to a Cloud SQL instance (Summarized in a decision tree for Best Practices)

<img src="../images/core_services_cloudSQL_connecting_to_CloudSQL_instance.png"
        alt="core_services_cloudSQL_connecting_to_CloudSQL_instance.png"
        style="float: left; margin-right: 10px;" />

Decision tree for your SQL storage

<img src="../images/core_services_cloudSQL_choosing_your_SQL_storage.png"
        alt="core_services_cloudSQL_choosing_your_SQL_storage.png"
        style="float: left; margin-right: 10px;" />


### Cloud SQL lab

- [lab tasks description](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/caD6h/lab-intro-cloud-sql)
- [lab notes](../labs/lab_Cloud_SQL.md)
- [Lab Review: Cloud SQL (MySQL)](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/1Il73/lab-review-cloud-sql)

<img src="../images/lab_Cloud_SQL_01.png"
        alt="lab_Cloud_SQL_01.png"
        style="float: left; margin-right: 10px;" />



### Cloud Spanner

[video](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/gu1Xc/cloud-spanner)

<img src="../images/lab_Cloud_Spanner_features.png"
        alt="lab_Cloud_Spanner_features.png"
        style="float: left; margin-right: 10px;" />

Best of Relational & non-relational DB world

<img src="../images/lab_Cloud_Spanner_characteristics_best_relational_non_relational_worlds.png"
        alt="lab_Cloud_Spanner_characteristics_best_relational_non_relational_worlds.png"
        style="float: left; margin-right: 10px;" />

This architecture allows for high availability and global placement:

<img src="../images/lab_Cloud_Spanner_architecture.png"
        alt="lab_Cloud_Spanner_architecture.png"
        style="float: left; margin-right: 10px;" />


<img src="../images/lab_Cloud_Spanner_Decision_tree_to_choose_Spanner.png"
        alt="lab_Cloud_Spanner_Decision_tree_to_choose_Spanner.png"
        style="float: left; margin-right: 10px;" />



### Cloud Firestore

[video](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/Z0QRZ/cloud-firestore)

Cloud Firestore is a:
- fast,
- fully managed,
- serverless Cloud native,
- NoSQL document database,
- that simplifies storing, sinking, and querying data for your mobile, web, and IoT apps at global scale

<img src="../images/lab_Cloud_Firestore.png"
        alt="lab_Cloud_Firestore.png"
        style="float: left; margin-right: 10px;" />


<img src="../images/lab_Cloud_Firestore_new_Datastore.png"
        alt="lab_Cloud_Firestore_new_Datastore.png"
        style="float: left; margin-right: 10px;" />

<img src="../images/lab_Cloud_FireStore_Decision_tree_to_choose_FireStore.png"
        alt="lab_Cloud_FireStore_Decision_tree_to_choose_FireStore.png"
        style="float: left; margin-right: 10px;" />


### Cloud Bigtable

[video](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/98vuS/cloud-bigtable)

<img src="../images/lab_Cloud_BigTable.png"
        alt="lab_Cloud_BigTable.png"
        style="float: left; margin-right: 10px;" />


Cloud Bigtable stores data in massively scalable tables:
- each of which is a sorted key value map.
- The table is composed of rows, each of which typically describes a single entity, - columns which contain individual values for each row.
- Each row is indexed by a single row key.
- Columns that are related to one another are typically grouped together into a column family.
- Each column is identified by a combination of the column family and a column qualifier which is a unique name within the column family.
- Each row column intersection can contain multiple cells or versions at different timestamps, providing a record of how the stored data has been altered over time. 


<img src="../images/lab_Cloud_BigTable_storage_model.png"
        alt="lab_Cloud_BigTable_storage_model.png"
        style="float: left; margin-right: 10px;" />

Processing is separated from storage:

<img src="../images/lab_Cloud_BigTable_storage_model_overal_architechture.png"
        alt="lab_Cloud_BigTable_storage_model_overal_architechture.png"
        style="float: left; margin-right: 10px;" />


Linear scaling of data re-distribution with number of nodes:

<img src="../images/lab_Cloud_BigTable_storage_model_overal_architechture_auto_rebalance.png"
        alt="lab_Cloud_BigTable_storage_model_overal_architechture_auto_rebalance.png"
        style="float: left; margin-right: 10px;" />

<img src="../images/lab_Cloud_BigTable_Decision_tree_to_choose_BigTable.png"
        alt="lab_Cloud_BigTable_Decision_tree_to_choose_BigTable.png"
        style="float: left; margin-right: 10px;" />



### Cloud Memorystore (Redis)

[video](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/Vh6cS/cloud-memorystore)


<img src="../images/lab_Cloud_MemoryStore_Redis.png"
        alt="lab_Cloud_MemoryStore_Redis.png"
        style="float: left; margin-right: 10px;" />


## Resource Management

= Manage and examine **Billing** of GCP resources

- [module overview](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/jPfvH/module-overview)

### Cloud Resource

- [video Resource Manager](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/ubZCA/cloud-resource-manager)

In this module, we will cover resource management.
Resources and GCP are billable, so **managing resources means controlling cost**. There are several methods in place for controlling access to the resources and there are quotas that limit consumption

Overview of Resource Manager ([video](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/ubZCA/cloud-resource-manager))

<img src="../images/lab_Cloud_ResourceManager.png"
        alt="lab_Cloud_ResourceManager.png"
        style="float: left; margin-right: 10px;" />

Root Node = Organization node:

<img src="../images/lab_Cloud_ResourceManager_root_node_organization.png"
        alt="lab_Cloud_ResourceManager_root_node_organization.png"
        style="float: left; margin-right: 10px;" />

From a physical organization standpoint, resources are categorized as:

- global,
- regional or
- zonal.

Let's look at some examples. Images, snapshots, and networks, are global resources. 

External IP addresses are regional resources, and instances and disks are zonal resources. However, regardless of the type, each resource is organized into a project. This enables each project to have its own billing and reporting.

<img src="../images/lab_Cloud_ResourceManager_what_can_be_tracked.png"
        alt="lab_Cloud_ResourceManager_what_can_be_tracked.png"
        style="float: left; margin-right: 10px;" />

<img src="../images/lab_Cloud_ResourceManager_regional_zonal.png"
        alt="lab_Cloud_ResourceManager_regional_zonal.png"
        style="float: left; margin-right: 10px;" />


### Quotas

[video](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/cVawh/quotas)

<img src="../images/lab_Resources_Quotas.png"
        alt="lab_Resources_Quotas.png"
        style="float: left; margin-right: 10px;" />

Why quotas?

- protection against high bills
- prevent billing spikes
- forces sizing considerations

<img src="../images/lab_Resources_Quotas_why.png"
        alt="lab_Resources_Quotas_why.png"
        style="float: left; margin-right: 10px;" />


### Labels and names

[video](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/kklUj/labels-and-names)

Projects and folders provide levels of segregation for resources. But what if you want more granularity?

That's where labels and names come in.

- **Labels are a utility for organizing GCP resources**.
- **Labels are key value pairs** that you can attach to your resources like VMs, disks, snapshots, and images.

<img src="../images/lab_Resources_Labels.png"
        alt="lab_Resources_Labels.png"
        style="float: left; margin-right: 10px;" />

What to use them for?

<img src="../images/lab_Resources_Labels_example_usage.png"
        alt="lab_Resources_Labels_example_usage.png"
        style="float: left; margin-right: 10px;" />


Labels vs Tags

<img src="../images/lab_Resources_Labels_vs_tags.png"
        alt="lab_Resources_Labels_vs_tags.png"
        style="float: left; margin-right: 10px;" />

### Billing

[video](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/PiRrk/billing)

<img src="../images/lab_Resources_budget_creation_interface.png"
        alt="lab_Resources_budget_creation_interface.png"
        style="float: left; margin-right: 10px;" />

#### Use Labels to optimize spendings

[video](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/PiRrk/billing)

Maybe these instances are sending most of their traffic to a different continent which could incur higher cost.

In that case, you might consider relocating some of those instances or using a caching service like Cloud CDN to cach content closer to your users, which reduces your networking spend. 

> **Google recommends labeling all your resources and exporting your billing data to BigQuery to analyze your spend.**

<img src="../images/lab_Resources_budget_use_Labels_to_optimize_spending.png"
        alt="lab_Resources_budget_use_Labels_to_optimize_spending.png"
        style="float: left; margin-right: 10px;" />

Visualize your spends with Data Studio

<img src="../images/lab_Resources_budget_visualize_spendings_over_time_DataStudio.png"
        alt="lab_Resources_budget_visualize_spendings_over_time_DataStudio.png"
        style="float: left; margin-right: 10px;" />

### Lab: Billing Administration

- [video billing administration](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/xpHII/demo-billing-administration)
- [video Lab Intro: Examining **Billing Data** with **BigQuery**](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/Nz755/lab-intro-examining-billing-data-with-bigquery)
- [Lab notes: Billing data with BigQuery](../labs/lab_billing_data_with_BigQuery.md)

## Resource Monitoring

=  monitor GCP resources using **StackDriver**

## Resources/Articles

- ["Programmatic Budgets Notification Examples"](https://cloud.google.com/billing/docs/how-to/notify)