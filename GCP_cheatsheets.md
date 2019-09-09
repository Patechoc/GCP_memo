# Google Cloud Platform Cheatsheets

* [General overview of GCP](#general-overview-of-gcp)
* [Tools](#tools)
   * [<code>gcloud</code> &amp; <code>gsutil</code>](#gcloud--gsutil)
   * [<code>kubectl</code>](#kubectl)
* [Storage](#storage)
* [Colaboratory](#colaboratory)
   * [Colab's history and inheritance](#colabs-history-and-inheritance)
* [GCP Products](#gcp-products)
   * [Dataproc](#dataproc)


## General overview of GCP

An interactive [map](https://coggle.it/diagram/W1hzfKfTAj0VEwcM/t/gcp-products) of GCP products
![products](./images/GCP_Products.png)


The Google Cloud Developer Cheat [GSheet](https://drive.google.com/open?id=1OkFbizpnc_iyzcApqRrqsNtUVazKJDtCyH5vw3352xM) ([Post on Medium](https://medium.com/google-cloud/the-google-cloud-developer-cheat-sheet-429775bd6d11))


![The Google Cloud Developer Cheat Sheet](https://cdn-images-1.medium.com/max/2000/1*ru3zUSy03sdJVkDD9_9Big.png)


GCP products described in 4 words or less ([Post on Medium](https://medium.com/google-cloud/gcp-products-described-in-4-words-or-less-f3056550e595))
![short definitions](https://cdn-images-1.medium.com/max/2000/1*uldXXOD-F4qKZpGMnFlcKw.png)


## Tools

### <code>gcloud</code> &amp; <code>gsutil</code>

* [dennyzhang/cheatsheet-gcp-A4](https://github.com/dennyzhang/cheatsheet-gcp-A4)
* quickstart by Google: https://cloud.google.com/storage/docs/quickstart-gsutil

### <code>kubectl</code>

* [dennyzhang/cheatsheet-kubernetes-A4](https://github.com/dennyzhang/cheatsheet-kubernetes-A4)
* [Official cheatsheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

## Storage

Choosing a storage option: https://cloud.google.com/storage-options/ (same decisions trees as below and a video with details)

![storage quick comparison](https://user-images.githubusercontent.com/300046/31043498-0da3e3a8-a57a-11e7-897e-8212079dcbfb.jpg)

|                  | Cloud Storage                  | Cloud Datastore               | Bigtable                           | Cloud SQL (1stGen)                 | BigQuery                           |
|------------------|--------------------------------|-------------------------------|------------------------------------|------------------------------------|------------------------------------|
| Storage type     | BLOB store                     | NoSQL, document               | wide column NoSQL                  | Relational SQL                     | Relational SQL                     |
| DB type          | Operational DB within an app   | Operational DB within an app  | Analytical DB in app or standalone | Operational DB within an app       | Analytical DB in app or standalone |
| Overall capacity | Petabytes+  (~10<sup>15</sup>) | Terabytes+ (~10<sup>12</sup>) | Petabytes+  (~10<sup>15</sup>)     | Up to 500 GB   (~0.5<sup>12</sup>) |                                    |
| Unit size        | 5 TB object                    | 1 MB entity                   | 10 MB cell                         | standard                           |                                    |
| Transactions     | No                             | Yes                           | No                                 | Yes                                |                                    |
| Complex queries  | No                             | No                            | No                                 | Yes                                |                                    |
| Billing          |                                |                               |                                    |                                    | NoOps, pay by operations, cheap storage |
| Strength         | *unstructured* data stores as *bytes* with a name within a place (.zip, .jpeg, .csv ) | NoSQL, no Schema & pay by the operation DB | DB storing +TBytes data, with millisec latency, high volume of writes, but expensive cluster, for apps that needs to scale | hosted MySQL service + Ops tasks.  | SQL query engine processing TBytes in seconds. Great for realtime analysis of large datasets |

![Storage decison tree](https://user-images.githubusercontent.com/300046/30033965-bd70b7a8-915b-11e7-9aba-9c2cbf5f62e8.png)

![Storage decison tree 2](https://i.stack.imgur.com/23CDj.png)

References:

* https://stackoverflow.com/questions/tagged/google-cloud-storage
* https://wilsonmar.github.io/gcp/ 


## Compute

* fully managed > **App Engine** (language specific: Ruby, PHP, .NET, Go, Java, Python, Node.js or any custom container with another language)
* Cluster management > **Kubernetes Engine**
* Moe control on HW > **Compute Engine**

## Colaboratory

### 

![colab](https://camo.qiitausercontent.com/0f3c4eabae42b14be76d89085d215d37971e918a/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f36343630382f64303862633335652d663632342d323339372d303137312d6333393365363431353662332e706e67)


### Colab's history and inheritance

![history & inheritance](https://camo.qiitausercontent.com/13c42c08aca1425cfe65ef95bfb1bb4666ebc7aa/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f36343630382f35326430373461302d653361612d363265362d306262372d6263366264663334376432612e706e67)

## GCP Products

### Dataproc

![Hadoop, Pig, Hive, Spark all in one](https://1.bp.blogspot.com/-5uC9aqnW1WQ/WO02q7GEh8I/AAAAAAAACJE/e4alCwjynh0hUFbVwHcvL8UJmgBWQa9aACLcB/w1200-h630-p-k-no-nu/dataproc.JPG)