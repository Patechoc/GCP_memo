# Summary of cloud services and respective open-source equivalent 


Edit this Sheet then convert to Markdown: https://docs.google.com/spreadsheets/d/1Ibo41Cs_XNdCFSqmNiXEQ16h-ZU6ATb6CZrMdKsLsZI

|  **** | **** | **Open-Source** | **GCP** | **AWS** | **...** |
| --- | --- | --- | --- | --- | --- |
|  **Compute** | IaaS |  | Compute Engine | Amazon Elastic Compute Cloud |  |
|   | PaaS |  |  |  |  |
|   | Containers |  | Google Kubernetes Engine | Amazon Elastic Container Service |  |
|   | Containers without infrastructure |  | Cloud Run | AWS Fargate |  |
|   | FaaS |  |  |  |  |
|   | Managed Batch Computing |  |  |  |  |
|  **Network** | Virtual Networks |  | Virtual Private Cloud | Amazon Virtual Private Cloud |  |
|   | Load Balancer |  | Cloud Load Balancing | Elastic Load Balancer |  |
|   | Dedicated Interconnect |  | Cloud Interconnect | Direct Connect |  |
|   | Domains and DNS |  | Google Domains, Cloud DNS | Amazon Route 53 |  |
|   | CDN |  | Cloud CDN | Amazon CloudFront |  |
|  **Storage** | Object Storage |  | Cloud Storage | Amazon Simple Storage Service |  |
|   | Block Storage |  |  |  |  |
|   | Reduced-availability Storage |  | Cloud Storage Nearline | Amazon S3 Standard-Infrequent Access, Amazon S3 One Zone-Infrequent Access |  |
|   | Archival Storage |  | Cloud Storage Coldline | Amazon Glacier |  |
|   | File Storage |  | Cloud Filestore (beta) | Amazon Elastic File System |  |
|  **Database** | RDBMS | MySQL | Cloud SQL, Cloud Spanner | Amazon Relational Database Service, Amazon Aurora |  |
|   | NoSQL: Key-value |  | [Cloud Firestore, Cloud Bigtable](https://cloud.google.com/firestore/ "Cloud Firestore, Cloud Bigtable") | Amazon DynamoDB |  |
|   | NoSQL: Indexed |  | Cloud Firestore | Amazon SimpleDB |  |
|  **Big Data & Analytics** | Batch Data Processing |  | Cloud Dataproc, Cloud Dataflow | Amazon Elastic MapReduce, AWS Batch |  |
|   | Stream Data Processing |  | Cloud Dataflow | Amazon Kinesis |  |
|   | Stream Data Ingest |  | Cloud Pub/Sub | Amazon Kinesis |  |
|   | Analytics |  | BigQuery | Amazon Redshift, Amazon Athena |  |
|   | Workflow Orchestration | Apache AirFlow | Cloud Composer | Amazon Data Pipeline, AWS Glue |  |
|  **Application Services** | Messaging | Apache Kafka | Cloud Pub/Sub | Amazon Simple Notification Service, Amazon Simple Queueing Service |  |
|  **Management Services** | Monitoring |  | Stackdriver Monitoring | Amazon CloudWatch |  |
|   | Logging |  | Stackdriver Logging | Amazon CloudWatch Logs |  |
|   | Deployment |  |  |  |  |
|  **Machine Learning** | Speech |  |  |  |  |
|   | Vision |  |  |  |  |
|   | Natural Language Processing |  |  |  |  |
|   | Translation |  |  |  |  |
|   | Conversational Interface |  |  |  |  |
|   | Video Intelligence |  |  |  |  |
|   | Auto-generated Models |  |  |  |  |
|   | Fully Managed ML |  |  |  |  |
|   |  |  |  |  |  |
|  **Infrastructure Deployment Tools** | infrastructure-as-code |  |  |  |  |
|  **Mobile** | Authentication |  |  |  |  |
|   | Database |  |  |  |  |
|   | Data Storage/CDN |  |  |  |  |
|   | Serverless routines |  |  |  |  |
|   | Notifications |  |  |  |  |
|   | Client application services |  |  |  |  |
## Resources/Articles

- [GCP vs AWS platforms](https://cloud.google.com/docs/compare/aws/#resource_management_interfaces)
- [Open-source Cloud tools](https://dzone.com/articles/top-5-enterprise-etl-tools)
- [Open-source tools matched with Cloud service providers](https://www.griddynamics.com/technologies/data-platform/cloud-native-data-platform)