# Memo for Building Resilient Streaming Systems on Google Cloud Platform

(by [COURSERA](https://www.coursera.org/learn/building-resilient-streaming-systems-gcp/home/welcome))

Learn how to build streaming **data pipelines** using Google Cloud **Pub/Sub** and **Dataflow** to enable real-time decision making.

You will also learn how to build **dashboards** to render tailored output for various stakeholder audience.


Objectives:

* Understand use-cases for real-time streaming analytics
* Use Google Cloud PubSub asynchronous messaging service to manage data events
* Write streaming pipelines and run transformations where necessary
* Get familiar with both sides of a streaming pipeline: production and consumption
* Interoperate Dataflow, BigQuery and Cloud Pub/Sub for real-time streaming and analysis

Modules:

1. Module 1: Architecture of Streaming Analytics Pipelines
1. Module 2: Ingesting Variable Volumes
1. Module 3: Implementing Streaming Pipelines
1. Module 4: Streaming analytics and dashboards
1. Module 5: Handling Throughput and Latency Requirements


## Table of Content

* [Content](#content)
   * [GCP Beam/Dataflow](#gcp-beamdataflow)
* [Lab 1 : Publish streaming data into Pub/Sub](#lab-1--publish-streaming-data-into-pubsub)
   * [Task 1: Preparation](#task-1-preparation)
   * [Task 2: Create Pub/Sub topic and subscription](#task-2-create-pubsub-topic-and-subscription)
   * [Task 3: Simulate traffic sensor data into Pub/Sub](#task-3-simulate-traffic-sensor-data-into-pubsub)
   * [Task 4: Verify that messages are received](#task-4-verify-that-messages-are-received)
* [Lab 2 : Streaming Data Pipelines](#lab-2--streaming-data-pipelines)
   * [Task 1: Preparation](#task-1-preparation-1)
   * [Task 2: Create a BigQuery Dataset and Cloud Storage Bucket](#task-2-create-a-bigquery-dataset-and-cloud-storage-bucket)
   * [Task 3: Simulate traffic sensor data into Pub/Sub](#task-3-simulate-traffic-sensor-data-into-pubsub-1)
   * [Task 4: Launch Dataflow Pipeline](#task-4-launch-dataflow-pipeline)
   * [Task 5: Explore the pipeline](#task-5-explore-the-pipeline)
   * [Task 6: Determine throughput rates](#task-6-determine-throughput-rates)
   * [Task 7: Review BigQuery output](#task-7-review-bigquery-output)
   * [Task 8: Observe and understand autoscaling](#task-8-observe-and-understand-autoscaling)
   * [Task 9: Refresh the sensor data simulation script](#task-9-refresh-the-sensor-data-simulation-script)
   * [Task 10: Stackdriver integration](#task-10-stackdriver-integration)
   * [Task 11: Explore metrics](#task-11-explore-metrics)
   * [Task 12: Create alerts](#task-12-create-alerts)
   * [Task 13: Set up dashboards](#task-13-set-up-dashboards)
   * [Task 14: Launch another streaming pipeline](#task-14-launch-another-streaming-pipeline)
* [references](#references)





## Content

* "So if you want to look for fraud transactions historically, well that's **batch**,"
* "but if you want to look at it as it happens, that **streaming**."



### GCP Beam/Dataflow

* **what** results are you calculating? (e.g. max() > P-transform in Dataflow) 
* **where** in "event time" are results calculated? (cf. via event-time windowing)
* **when** in processing time are results materialized? (cf. watermarks, triggers and allowed lateness)
* **how** do you refine when a message comes late? (cf. accumulation modes)

https://cloud.google.com/blog/products/gcp/after-lambda-exactly-once-processing-in-google-cloud-dataflow-part-1

So, these powerful out-of-order processing constructs, watermarks, triggers, they all give you the ability to maintain eventual correctness of results in a pipeline.

But at the same time, they give you low latency speculation (calculate a max() before all the data are in. If a message less then 3 hours late, I wil refine my result, more, I will ignore it!).



To summarize:

* **Pub/Sub** is a global message bus.
* **Dataflow** is capable of doing batch and streaming, the core doesn't change. It gives you the better deal with late data and unordered data,
* **Big Query** gives you the power of doing analytics both on historical data and on streaming data. Later we will also look at an _*alternative sink*_ which is not BigQuery but Bigtable. If you need very high throughput, very low latency, you can use **Big Table** as an alternate solution.



## Lab 1 : Publish streaming data into Pub/Sub

In this lab you will use simulate your traffic sensor data into a Pub/Sub topic for later to be processed by Dataflow pipeline before finally ending up in a BigQuery table for further analysis


First you will:

* Create a Pub/Sub topic and subscription
* Simulate your traffic sensor data into Pub/Sub

> At the time of this writing, streaming pipelines are not available in the DataFlow Python SDK. So the streaming labs are written in Java.

* Click Open [Google Console](https://console.cloud.google.com).


### Task 1: Preparation

You will be running a sensor simulator from the training VM. There are several files and some setup of the environment required.

Open the SSH terminal and connect to the training VM

* In the Console, on the Navigation menu, click **Compute Engine > VM instances**.
* Locate the line with the instance called **training_vm**.
* On the far right, under 'connect', Click on **SSH** to open a terminal window.
* In this lab you will enter CLI commands on the **training_vm**.

Verify initialization is complete

* The **training_vm** is installing software in the background. Verify that setup is complete by checking that the following directory exists. If it does not exist, wait a few minutes and try again.

```shell
google849263_student@training-vm:~$ ls /training
env.txt  project_env.sh  Project_ID  sensor_magic.sh  training-data-analyst
google849263_student@training-vm:~$ 
```

* Wait until setup is complete before proceeding. You can verify the installation of maven with `mvn -version` and the JDK with `java -version`.

```shell
google849263_student@training-vm:~$ date
Wed Aug 15 13:49:31 UTC 2018
google849263_student@training-vm:~$ mvn -version
-bash: mvn: command not found
google849263_student@training-vm:~$ java -version
-bash: java: command not found
```


Copy files

* A repository has been downloaded to the VM. Copy the repository to your home directory.

```shell
cp -r /training/training-data-analyst/ .
```

Identify a project

* One environment variable that you will set is **$DEVSHELL_PROJECT_ID** that contains the Google Cloud project ID required to access billable resources.

```shell
google849263_student@training-vm:~$ echo $DEVSHELL_PROJECT_ID
google849263_student@training-vm:~$ export DEVSHELL_PROJECT_ID="qwiklabs-gcp-6a28a8c8fbb74f6e"
google849263_student@training-vm:~$ echo $DEVSHELL_PROJECT_ID
qwiklabs-gcp-6a28a8c8fbb74f6e
google849263_student@training-vm:~$
```



### Task 2: Create Pub/Sub topic and subscription

* On the **training_vm** SSH terminal, navigate to the directory for this lab.


```shell
cd ~/training-data-analyst/courses/streaming/publish
```

Verify that the Pub/Sub service is accessible and working using the gcloud command.

* Create your topic and publish a simple message.

```shell
gcloud pubsub topics create sandiego
Created topic [projects/qwiklabs-gcp-6a28a8c8fbb74f6e/topics/sandiego].
```


* Publish a simple message.

```shell
gcloud pubsub topics publish sandiego --message "hello"
messageIds:
- '169571988670546'
```

* Create a subscription for the topic.

```shell
gcloud pubsub subscriptions create --topic sandiego mySub1
Created subscription [projects/qwiklabs-gcp-6a28a8c8fbb74f6e/subscriptions/mySub1].
```

* Pull the first message that was published to your topic.

```shell
gcloud pubsub subscriptions pull --auto-ack mySub1
Listed 0 items.
```

**Do you see any result? If not, why?**

I wasn't subscribed at the time the first message was sent.

* Try to publish another message and then pull it using the subscription.

```shell
gcloud pubsub topics publish sandiego --message "hello again"
messageIds:
- '169574808195471'
gcloud pubsub subscriptions pull --auto-ack mySub1
┌─────────────┬─────────────────┬────────────┐
│     DATA    │    MESSAGE_ID   │ ATTRIBUTES │
├─────────────┼─────────────────┼────────────┤
│ hello again │ 169574808195471 │            │
└─────────────┴─────────────────┴────────────┘
google849263_student@training-vm:~$
```

**Did you get any response this time?**

Yes ;)



* Return to the Console tab. On the Navigation menu,click **Pub/Sub > Topics**.

* You should see a line with the **Topic Name** ending in sandiego and the number of Subscriptions set to 1.

* In the **training_vm** SSH terminal, cancel your subscription.

```shell
gcloud pubsub subscriptions delete mySub1
Deleted subscription [projects/qwiklabs-gcp-6a28a8c8fbb74f6e/subscriptions/mySub1].
```

* Return to the Console tab. Refresh the browser and you should see the Subscriptions drop to 0.


### Task 3: Simulate traffic sensor data into Pub/Sub


* Explore the python script to simulate San Diego traffic sensor data. Do **not make any changes to the code**.

```shell
cd ~/training-data-analyst/courses/streaming/publish
less send_sensor_data.py
```

Look at the simulate function. This one lets the script behave as if traffic sensors were sending in data in real time to Pub/Sub. The speedFactor parameter determines how fast the simulation will go.

* Download the traffic simulation dataset ([download_data.sh](https://github.com/Patechoc/training-data-analyst/blob/master/courses/streaming/publish/download_data.sh)).

```shell
./download_data.sh
Copying gs://cloud-training-demos/sandiego/sensor_obs2008.csv.gz...
/ [1 files][ 34.6 MiB/ 34.6 MiB]                                                
Operation completed over 1 objects/34.6 MiB.
```


Install API support

* Install the Python PIP program required to install the API.

```shell
sudo apt-get install -y python-pip
Reading package lists... Done
Building dependency tree       
Reading state information... Done
python-pip is already the newest version (9.0.1-2).
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

* Use PIP to install the Google Cloud Pub/Sub API.

```shell
sudo pip install -U google-cloud-pubsub
...
Requirement already up-to-date: pyasn1<0.5.0,>=0.4.1 in /usr/local/lib/python2.7/dist-packages (from pyasn1-modules
>=0.2.1->google-auth<2.0.0dev,>=0.4.0->google-api-core[grpc]<2.0.0dev,>=1.1.0->google-cloud-pubsub)
```

Simulate streaming sensor data

* Run the [send_sensor_data.py](https://github.com/Patechoc/training-data-analyst/blob/master/courses/streaming/publish/send_sensor_data.py)

```shell
./send_sensor_data.py --speedFactor=60 --project $DEVSHELL_PROJECT_ID
INFO: Reusing pub/sub topic sandiego
INFO: Sending sensor data from 2008-11-01 00:00:00
INFO: Publishing 477 events from 2008-11-01 00:00:00
INFO: Sleeping 5.0 seconds
INFO: Publishing 477 events from 2008-11-01 00:05:00
INFO: Sleeping 5.0 seconds
INFO: Publishing 477 events from 2008-11-01 00:10:00
INFO: Sleeping 5.0 seconds
INFO: Publishing 477 events from 2008-11-01 00:15:00
INFO: Sleeping 5.0 seconds
...
```

This command simulates sensor data by sending recorded sensor data via Pub/Sub messages. 
The script extracts the original time of the sensor data and pauses between sending each message to simulate realistic timing of the sensor data. 
The value **speedFactor** changes the time between messages proportionally. So a **speedFactor** of 60 means '60 times faster' than the recorded timing. It will send about an hour of data every 60 seconds.

Leave this terminal open and the simulator running.


### Task 4: Verify that messages are received

Open a second SSH terminal and connect to the training VM

* In the Console, on the **Navigation** menu, click **Compute Engine > VM instances**.
* Locate the line with the instance called **training_vm**.
* On the far right, under 'connect', Click on **SSH** to open a second terminal window.
* Change into the directory you were working in:

```shell
cd ~/training-data-analyst/courses/streaming/publish
export DEVSHELL_PROJECT_ID="qwiklabs-gcp-6a28a8c8fbb74f6e"
```

* Create a subscription for the topic and do a pull to confirm that messages are coming in:

```shell
gcloud pubsub subscriptions create --topic sandiego mySub2
Created subscription [projects/qwiklabs-gcp-6a28a8c8fbb74f6e/subscriptions/mySub2].

gcloud pubsub subscriptions pull --auto-ack mySub2
┌────────────────────────────────────────────────────────┬─────────────────┬────────────┐
│                          DATA                          │    MESSAGE_ID   │ ATTRIBUTES │
├────────────────────────────────────────────────────────┼─────────────────┼────────────┤
│ 2008-11-01 03:15:00,32.749679,-117.155519,163,S,1,69.9 │ 169580771629017 │            │
└────────────────────────────────────────────────────────┴─────────────────┴────────────┘
```

Confirm that you see a message with traffic sensor information.

* Cancel this subscription.

```shell
gcloud pubsub subscriptions delete mySub2
Deleted subscription [projects/qwiklabs-gcp-6a28a8c8fbb74f6e/subscriptions/mySub2].
```

* Close the second terminal.

```shell
exit
```

Stop the sensor simulator

* Return to the first terminal.
* Interrupt the publisher by typing **Ctrl-C** to stop it.
* Close the first terminal.

```shell
exit
```


----------------------------------------------------------------------------------
## Dataflow Streaming: Implementing streaming pipelines (module #3)

### Definitions

[video](https://www.coursera.org/learn/building-resilient-streaming-systems-gcp/lecture/a3LsQ/handle-late-data-watermarks-triggers-accumulation)

**Event time**: time at which the event occured.

**Processing time**: time at which we get to process this event.

* **what** are you computing? (**Transformations**)
* **where** in "event time"  (**Windowing**)
* **when** in "processing time" (**Watermarks + Triggers**)
* **how** do you refine when a message comes late? (**Accumulation**)

**Watermark**: is the tracker measuring how far the processing time is from the event time.


----------------------------------------------------------------------------------

## Lab 2 : Streaming Data Pipelines

[lab demo review in video](https://www.coursera.org/learn/building-resilient-streaming-systems-gcp/lecture/kPEBt/lab-demo-and-review)

> At the time of this writing, streaming pipelines are not available in the DataFlow Python SDK. So the streaming labs are written in Java.

In this lab you will use Dataflow to collect traffic events from simulated traffic sensor data made available through Google Cloud PubSub, process them into an actionable average, and store the raw data in BigQuery for later analysis. You will learn how to start a Dataflow pipeline, monitor it, and, lastly, optimize it.

In this lab you:

* Launch Dataflow and run a Dataflow job
* Understand how data elements flow through the transformations of a Dataflow pipeline
* Connect Dataflow to Pub/Sub and BigQuery
* Observe and understand how Dataflow autoscaling adjusts compute resources to process input data optimally
* Learn where to find logging information created by Dataflow
* Explore metrics and create alerts and dashboards with Stackdriver Monitoring




### Task 1: Preparation

You will be running a sensor simulator from the training VM. In Lab 1 you manually setup the Pub/Sub components. In this lab several of those process are automated.

Open the SSH terminal and connect to the training VM

* In the Console, on the Navigation menu, click **Compute Engine > VM instances**.
* Locate the line with the instance called **training_vm**.
* On the far right, under 'connect', Click on **SSH** to open a terminal window.
* In this lab you will enter CLI commands on the **training_vm**.

Verify initialization is complete

* The **training_vm** is installing software in the background. Verify that setup is complete by checking that the following directory exists. If it does not exist, wait a few minutes and try again.

```shell
google862329_student@training-vm:~$ ls /training
env.txt  project_env.sh  Project_ID  sensor_magic.sh  training-data-analyst
google849263_student@training-vm:~$ 
```

* Wait until setup is complete before proceeding. You can verify the installation of maven with `mvn -version` and the JDK with `java -version`.

```shell
google862329_student@training-vm:~$ mvn -version
Apache Maven 3.3.9
Maven home: /usr/share/maven
Java version: 1.8.0_181, vendor: Oracle Corporation
Java home: /usr/lib/jvm/java-8-openjdk-amd64/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "4.9.0-7-amd64", arch: "amd64", family: "unix"
google862329_student@training-vm:~$ java -version
openjdk version "1.8.0_181"
OpenJDK Runtime Environment (build 1.8.0_181-8u181-b13-1~deb9u1-b13)
OpenJDK 64-Bit Server VM (build 25.181-b13, mixed mode)
google862329_student@training-vm:~$```


Copy files

* A repository has been downloaded to the VM. Copy the repository to your home directory.

```shell
cp -r /training/training-data-analyst/ .
```

Set environment variables

* On the **training_vm** SSH terminal enter the following:

```shell
google862329_student@training-vm:~$ cat /training/project_env.sh 
#! /bin/bash

# Create the DEVSHELL_PROJECT_ID on a VM
curl "http://metadata.google.internal/computeMetadata/v1/project/project-id" -H "Metadata-Flavor: Google" > Project_ID
awk '{print "export DEVSHELL_PROJECT_ID=" $0, "\n" "export BUCKET=" $0, "\n" "export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre" }' Project_ID > env.txt
source env.txt
echo $DEVSHELL_PROJECT_ID

google862329_student@training-vm:~$ 
google862329_student@training-vm:~$ source /training/project_env.sh
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    29  100    29    0     0   2525      0 --:--:-- --:--:-- --:--:--  2636
qwiklabs-gcp-857b9f8302e9ff3b
```

This script sets the **$DEVSHELL_PROJECT_ID** and **$BUCKET** environment variables.

### Task 2: Create a BigQuery Dataset and Cloud Storage Bucket

The Dataflow pipeline will be created later and will write into a table in this dataset.

Create a BigQuery Dataset

* Open the BigQuery web UI. On the **Navigation** menu, click **BigQuery**.
* In the left column, beneath the text box, find your project name. To the right of the project name, click the **blue arrow**. Choose **Create new dataset**.

![create new dataset](https://run-qwiklab-website-prod.s3.amazonaws.com/instructions/documents/52721/original/img/502c44e195ac5c74.png)

* In the ‘Create Dataset' dialog, for **Dataset ID**, type **demos** and click OK.


Verify the Cloud Storage Bucket

A bucket should already exist that has the same name as the Project ID.

* In the Console, on the **Navigation** menu click **Storage > Browser**.

* Observe the following values:

| Property  | Value  |
|---|---|
| Name  | qwiklabs-gcp-857b9f8302e9ff3b (<Same as the Project ID>)  |
| Default storage class  | Regional  |
| Multi-REgional location  |  (<Your location>) |

QL Region: us-central1

QL Zone: us-central1-a


### Task 3: Simulate traffic sensor data into Pub/Sub

* In the **training_vm** SSH terminal, start the sensor simulator. The script reads sample data from a csv file and publishes it to Pub/Sub.

```shell
/training/sensor_magic.sh
```

This command will send 1 hour of data in 1 minute. Let the script continue to run in the current terminal.

Open a second SSH terminal and connect to the training VM

* In the upper right corner of the **training_vm** SSH terminal, click on the gear-shaped button (![button](https://run-qwiklab-website-prod.s3.amazonaws.com/instructions/documents/52721/original/img/9649d58acf1c4e06.png)) and select **New Connection to training-vm** from the drop-down menu. A new terminal window will open.

![window](https://run-qwiklab-website-prod.s3.amazonaws.com/instructions/documents/52721/original/img/aaafeb5bff33d832.png)

* The new terminal session will not have the required environment variables. Run the following command to set them.
* In the new **training_vm** SSH terminal enter the following:
* In the Console, on the Navigation menu () click Pub/Sub>Topics
* Examine the line for **Topic name** for the topic **sandiego**. Notice that **Subscriptions** are currently at 0.

```shell
source /training/project_env.sh
```

View subscriptions

* In the Console, on the **Navigation** menu, click **Pub/Sub>Topics**
* Examine the line for **Topic name** for the topic **sandiego**. Notice that **Subscriptions** are currently at 0.

### Task 4: Launch Dataflow Pipeline

Verify that Google Cloud **Dataflow API** is enabled for this project

* Return to the browser tab for Console. In the top search bar, enter Dataflow API. This will take you to the page, **Navigation > APIs & Services > Dashboard > Google Dataflow API**. It will either show a status information or it will give you the option to **Enable** the API.
* If necessary, Enable the API.
* Return to the second **training_vm** SSH terminal. Change into the directory for this lab.

```shell
cd ~/training-data-analyst/courses/streaming/process/sandiego
```

* Identify the script that creates and runs the Dataflow pipeline ([`run_oncloud.sh`](https://github.com/Patechoc/training-data-analyst/blob/master/courses/streaming/process/sandiego/run_oncloud.sh)).

```shell
cat run_oncloud.sh
```

* Copy-and-paste the following URL into a new browser tab to view the source code on Github.

  [https://github.com/Patechoc/training-data-analyst/blob/master/courses/streaming/process/sandiego/run_oncloud.sh](https://github.com/Patechoc/training-data-analyst/blob/master/courses/streaming/process/sandiego/run_oncloud.sh)

* The script requires three arguments: **project id**, **bucket name**, **classname**

A 4th optional argument is **options**. The **options** argument in discussed later in this lab.

| project id | <your Project ID> |
| bucket name | <your Bucket Name> |
| classname |  <java file that runs aggregations> |
| options | <options> |

There are 4 java files that you can choose from for **classname**. Each reads the traffic data from Pub/Sub and runs different aggregations/computations.

* Go into the java directory. Identify the source file [**AverageSpeeds.java**](https://github.com/Patechoc/training-data-analyst/blob/master/courses/streaming/process/sandiego/src/main/java/com/google/cloud/training/dataanalyst/sandiego/AverageSpeeds.java). **This is THE program building the Dataflow pipeline!!!!**

```shell
cd ~/training-data-analyst/courses/streaming/process/sandiego/src/main/java/com/google/cloud/training/dataanalyst/sandiego 

cat AverageSpeeds.java
```

What does the script do?

Close the file to continue. You will want to refer to this source code while running the application. So for easy access you will open a new browser tab and view the file **AverageSpeeds.java** on Github.

* Copy-and-paste the following URL into a browser tab to view the source code on Github.

`https://github.com/Patechoc/training-data-analyst/blob/master/courses/streaming/process/sandiego/src/main/java/com/google/cloud/training/dataanalyst/sandiego/AverageSpeeds.java`

Leave this browser tab open. You will be referring back to the source code in a later step in this lab.

* Return to the **training_vm** SSH terminal. Run the Dataflow pipeline to read from PubSub and write into BigQuery.
```shell
cd ~/training-data-analyst/courses/streaming/process/sandiego

./run_oncloud.sh $DEVSHELL_PROJECT_ID $BUCKET AverageSpeeds
```

This script uses maven to build a Dataflow streaming pipeline in Java.

Example successful completion:

```shell
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 45.542 s
[INFO] Finished at: 2018-06-08T16:51:30+00:00
[INFO] Final Memory: 56M/216M
[INFO] ------------------------------------------------------------------------
```


### Task 5: Explore the pipeline


This Dataflow pipeline reads messages from a Pub/Sub topic, parses the JSON of the input message, produces one main output and writes to BigQuery.

* Return to the browser tab for Console. On the Navigation menu () click Dataflow and click on your job to monitor progress.

Example:

![img](https://run-qwiklab-website-prod.s3.amazonaws.com/instructions/documents/52721/original/img/1540d0eba03f0a0a.png)

* After the pipeline is running, click on the **Navigation** menu, click **Pub/Sub>Topics**.
* Examine the line for **Topic name** for the topic **sandiego**. Notice that **Subscriptions** field is now at 1.

Example:

![img](https://run-qwiklab-website-prod.s3.amazonaws.com/instructions/documents/52721/original/img/a52cd9a54fc0e6ec.png)

* Return to the **Navigation** menu, click **Dataflow** and click on your job.
* Compare the code in the Github browser tab, **AverageSpeeds.java** and the pipeline graph in the page for your Dataflow job.
* Find the **"GetMessages"** pipeline step in the graph, and then find the corresponding code in the **AverageSpeeds.java** file. This is the pipeline step that reads from the Pub/Sub topic. It creates a collection of Strings - which corresponds to Pub/Sub messages that have been read.

    * Do you see a subscription created?
    * How does the code pull messages from Pub/Sub?
* Find the **"Time Window"** pipeline step in the graph and in code. In this pipeline step we create a window of a duration specified in the pipeline parameters (sliding window in this case). This window will accumulate the traffic data from the previous step until end of window, and pass it to the next steps for further transforms.

    * What is the window interval?
    * How often is a new window created?
* Find the **"BySensor"** and **"AvgBySensor"** pipeline steps in the graph, and then find the corresponding code snippet in the **AverageSpeeds.java** file. This **"BySensor"** does a grouping of all events in the window by sensor id, while **"AvgBySensor"** will then compute the mean speed for each grouping.
* Find the **"ToBQRow"** pipeline step in the graph and in code. This step simply creates a "row" with the average computed from previous step together with the lane information.

    > In practice, other actions could be taken in the **ToBQRow** step. For example, it could compare the calculated mean against a predefined threshold and log the results of the comparison in Stackdriver Logging.

* Find the **"BigQueryIO.Write"** in both the pipeline graph and in the source code. This step writes the row out of the pipeline into a BigQuery table. Because we chose the **WriteDisposition.WRITE_APPEND** write disposition, new records will be appended to the table.
* Return to the BigQuery web UI tab Or open it from the **Navigation** menu, click **BigQuery**. Refresh your browser.
* In the left column, beneath the text box, find your project name and the demos dataset you created. The small blue arrow to the left should now be active and clicking on it will reveal the **average_speeds** table.

Example:

![img](https://run-qwiklab-website-prod.s3.amazonaws.com/instructions/documents/52721/original/img/57a35c3363b38ffc.png)

### Task 6: Determine throughput rates

One common activity when monitoring and improving Dataflow pipelines is figuring out how many elements the pipeline processes per second, what the system lag is, and how many data elements have been processed so far. In this activity you will learn where in the Cloud Console one can find information about processed elements and time.

* Return to the browser tab for Console. On the **Navigation** menu. click **Dataflow** and click on your job to monitor progress (it will have your username in the pipeline name).
* Select the **"GetMessages"** pipeline node in the graph and look at the step metrics on the right.
    * **System Lag** is an important metric for streaming pipelines. It represents the amount of time data elements are waiting to be processed since they "arrived" in the input of the transformation step.
    * **Elements Added** metric under output collections tells you how many data elements exited this step (for the **"Read PubSub Msg"** step of the pipeline it also represents the number of Pub/Sub messages read from the topic by the Pub/Sub IO connector).
* Select the **"Time Window"** node in the graph. Observe how the Elements Added metric under the Input Collections of the **"Time Window"** step matches the Elements Added metric under the Output Collections of the previous step **"GetMessages"**.



### Task 7: Review BigQuery output


* Return to the BigQuery web UI or on the Navigation menu () click BigQuery.

> Streaming data and tables may not show up immediately, and the **Preview** feature may not be available for data that is still in the streaming buffer. If you click on Preview you will see the message "This table has records in the streaming buffer that may not be visible in the preview." You can still run queries to view the data.

* Use the following query to observe the output from the Dataflow job. Replace <PROJECTID> with your Project ID. It is listed under connection details in Qwiklabs.

```sql
SELECT * 
FROM [<PROJECTID>:demos.average_speeds] 
ORDER BY timestamp DESC
LIMIT 100
```

* Find the last update to the table by running the following SQL.

```sql
SELECT
  MAX(timestamp)
FROM
  [<PROJECTID>:demos.average_speeds]
```

* Use the BigQuery Table Decorator to look at results in the last 10 minutes.

```sql
SELECT
  *
FROM
  [<PROJECTID>:demos.average_speeds@-600000]
ORDER BY
  timestamp DESC
```

* Use the **BigQuery Invalid Snapshot Time**, try reducing the 600000 to 100000.



### Task 8: Observe and understand autoscaling

Observe how Dataflow scales the number of workers to process the backlog of incoming Pub/Sub messages.

* Return to the browser tab for Console. On the **Navigation** menu, click **Dataflow** and click on your pipeline job.
* Examine the **Job summary** panel on the right, and review the **Autoscaling** section. How many workers are currently being used to process messages in the Pub/Sub topic?
* Click on **"See more history"** and review how many workers were used at different points in time during pipeline execution.
* The data from a traffic sensor simulator started at the beginning of the lab creates hundreds of messages per second in the Pub/Sub topic. This will cause Dataflow to increase the number of workers to keep the system lag of the pipeline at optimal levels.
* Click on **See more history**. In the **Worker History** pop-up, you can see how Dataflow changed the number of workers. Notice the **Rationale** column that explains the reason for the change.


### Task 9: Refresh the sensor data simulation script

> The training lab environment has quota limits. If the sensor data simulation script runs too long it will pass a quota limit, causing the session credentials to be suspended.


* Return to the **training_vm** SSH terminal where the sensor data is script is running.
* If you see messages that say **"INFO: Publishing"** then the script is still running. Press CRTL-C to stop it. Then issue the command to start the script again.

```shell
cd ~/training-data-analyst/courses/streaming/publish

./send_sensor_data.py --speedFactor=60 --project $DEVSHELL_PROJECT_ID
```

* If the script has passed the quota limit, you will see repeating error messages that "credentials could not be refreshed" and you may not be able to use **CTRL-C** to stop the script. Simply close the SSH terminal. Open a new SSH terminal. The new session will have a fresh quota.
* In the Console, on the **Navigation** menu, click **Compute Engine > VM instances**.
* Locate the line with the instance called **training_vm**.
* On the far right, under 'connect', Click on **SSH** to open a second terminal window.
* In the **training_vm** SSH terminal, enter the following to create environment variables.

```shell
source /training/project_env.sh
```

* Use the following commands to start a new sensor simulator.

```shell
cd ~/training-data-analyst/courses/streaming/publish

./send_sensor_data.py --speedFactor=60 --project $DEVSHELL_PROJECT_ID
```

### Task 10: Stackdriver integration

Stackdriver Monitoring integration with Dataflow allows users to access Dataflow job metrics such as System Lag (for streaming jobs), Job Status (Failed, Successful), Element Counts, and User Counters from within Stackdriver.

Monitoring Integration features of Stackdriver

* **Explore Dataflow Metrics**: Browse through available Dataflow pipeline metrics and visualize them in charts.

Some common Dataflow metrics.

| Job status | Job status (Failed, Successful), reported as an enum every 30 secs and on update. |
| Elapsed time | Job elapsed time (measured in seconds), reported every 30 secs. |
| System lag | Max lag across the entire pipeline, reported in seconds. |
| Current vCPU count | Current # of virtual CPUs used by job and updated on value change. |
| Estimated byte count | Number of bytes processed per PCollection. |

* **Chart Dataflow metrics in Stackdriver Dashboards**: Create Dashboards and chart time series of Dataflow metrics.
* **Configure Alerts**: Define thresholds on job or resource group-level metrics and alert when these metrics reach specified values. Stackdriver alerting can notify on a variety of conditions such as long streaming system lag or failed jobs.
* **Monitor User-Defined Metrics**: In addition to Dataflow metrics, Dataflow exposes user-defined metrics (SDK Aggregators) as Stackdriver custom counters in the Monitoring UI, available for charting and alerting. Any Aggregator defined in a Dataflow pipeline will be reported to Stackdriver as a custom metric. Dataflow will define a new custom metric on behalf of the user and report incremental updates to Stackdriver approximately every 30 seconds.



### Task 11: Explore metrics with StackDriver

Stackdriver monitoring is a separate service in Google Cloud Platform. So you will need to go through some setup steps to initialize the service for your lab account.

Setup Stackdriver account

* Return to the browser tab for Console. On the **Navigation** menu, click **Stackdriver > Monitoring**.
* Click **Log in with Google**.
* Click **Create Account**.
* Click **Continue**.
* Click **Skip AWS Setup**.
* Click **Continue**.
* Select **No Reports** and click **Continue**.
* It may take a few minutes for Stackdriver to import project information about your lab account and the resources already being used. Once the **Launch monitoring** button becomes active, click Launch monitoring.
* The trial version of Stackdriver provides the **Premium Tier** of service. So upgrading simply sets up billing so the account will not revert to Basic Tier at the end of 30 days.
* Click on **Continue with the trial**. (You can also click on 'Dismiss' on the message bar at the top asking if you want to upgrade).

Explore Stackdriver Metrics

* In the panel to the left click on **Resources > Metrics Explorer**
* In the Metrics Explorer, find and select the **Dataflow_job** resource type. You should see a list of available Dataflow-related metrics.

![img](https://run-qwiklab-website-prod.s3.amazonaws.com/instructions/documents/52721/original/img/5eb4e8a59b5430c9.png)

* Select the resource **Dataflow Job** and the metric **Data watermark lag**.
* Stackdriver will draw a graph on the right side of the page.
* Under **Find resource type and metric**, click on the (**x**) to remove the **Data watermark lag** metric. Select a new metric, **System Lag**.

> The metrics that Dataflow provides to Stackdriver are listed here:
> 
> https://cloud.google.com/monitoring/api/metrics_gcp
> 
> (Search on the page for Dataflow).
> 
> The metrics you have viewed are useful indicators of pipeline performance.
> 
> **Data watermark age**: The age (time since event timestamp) of the most recent item of data that has been fully processed by the pipeline.
> 
> **System lag**: The current maximum duration that an item of data has been awaiting processing, in seconds.

### Task 12: Create alerts with StackDriver

If you want to be notified when a certain metric crosses a specified threshold (for example, when System Lag of our lab streaming pipeline increases above a predefined value), you could use the Alerting mechanisms of Stackdriver to accomplish that.

Create an alert

* On the Stackdriver Monitoring click on **Stackdriver > Alerting > Policies Overview**.
* Click on **Add Policy**.
* On the **Create new Alerting Policy** page click on **Add Condition**.
* On the **Metric Threshold** row, click **Select**.
* In the **Target** section, set the **RESOURCE TYPE** to **Dataflow Job**.
* Under **APPLIES TO**, select **Single**.
* Select the resource you used in the previous task.
* In the **Configuration** section, set **IF METRIC** to **System Lag**.
* Set **CONDITION** to **above**.
* Set **THRESHOLD** to `5`
* Set **FOR** to `1 minute`.
* Click on **Save Condition** to save the alert.

Add a notification

* Under **Notification**, click on the pulldown menu to view the options for notification channel. You can set up a notification policy if you would like, using your email address.
* In the **Name this policy** section, give the policy a name such as **MyAlertPolicy**.
* Click on **Save Policy**.

View events

* On the Stackdriver tab, click on **Alerting > Events**.
* Every time an alert is triggered by a Metric Threshold condition, an Incident and a corresponding Event are created in Stackdriver. If you specified a notification mechanism in the alert (email, SMS, pager, etc), you will also receive a notification.


### Task 13: Set up dashboards with StackDriver

You can easily build dashboards with the most relevant Dataflow-related charts with Stackdriver Monitoring Dashboards.

* On the Stackdriver tab, click on **Dashboards > Create dashboard**.
* Click on **Add Chart**.
* On the Add Chart page:
* In the **Find resource type and metric** box, start typing **Dataflow Job** and then select it as the Resource Type.
* After you select a Resource Type, the Metric field menu will appear. Select a metric to chart, such as **System Lag**.
* in the Filter panel, select **project**, then the equals sign '=', then your Project ID.
* click **Save**.

Example:

![img](https://run-qwiklab-website-prod.s3.amazonaws.com/instructions/documents/52721/original/img/ad665cd6a43d2ffd.png)


You can add more charts to the dashboard, if you would like, for example, Pub/Sub publish rates on the topic, or subscription backlog (which is a signal to the Dataflow auto-scaler).


### Task 14: Launch another streaming pipeline

* In the **training_vm** SSH terminal, examine the **CurrentConditions.java** application.

> Do not make any changes to the code.

```shell
cd ~/training-data-analyst/courses/streaming/process/sandiego/src/main/java/com/google/cloud/training/dataanalyst/sandiego 

cat CurrentConditions.java
```

* Copy-and-paste the following URL into a browser tab to view the source code on Github.

`https://github.com/Patechoc/training-data-analyst/blob/master/courses/streaming/process/sandiego/src/main/java/com/google/cloud/training/dataanalyst/sandiego/CurrentConditions.java`

What does the script do?

* Run the **CurrentConditions.java** code in a new Dataflow pipeline; this script is simpler in the sense that it does not do many transforms like **AverageSpeeds**. The results will be used in the next lab to build dashboards and run some transforms (functions) while retrieving data from BigQuery.
* In the training_vm SSH terminal, enter the following:

```shell
cd ~/training-data-analyst/courses/streaming/process/sandiego

./run_oncloud.sh $DEVSHELL_PROJECT_ID $BUCKET CurrentConditions
```

* Return to the browser tab for Console. On the **Navigation** menu, click **Dataflow** and click on the new pipeline job. Confirm that the pipeline job is listed and verify that it is running without errors.
* It will take several minutes before the **current_conditions** table appears in BigQuery.


----------------------------------------------------------------------------------

## references

* [lab demo review in video](https://www.coursera.org/learn/building-resilient-streaming-systems-gcp/lecture/kPEBt/lab-demo-and-review)
* [GCP Dataflow examples in Python (and JAVA)](https://github.com/GoogleCloudPlatform/DataflowSDK-examples)
* [Codes Samples & Walkthoughs of applied use cases](https://cloud.google.com/dataflow/docs/samples)
  * [Machine Learning with Apache Beam and TensorFlow](https://cloud.google.com/dataflow/examples/molecules-walkthrough
  * [Examples for the Apache Beam SDKs](https://cloud.google.com/dataflow/examples/examples-beam)
    * [Wordcount Walkthrough](https://beam.apache.org/get-started/wordcount-example/): a series of four successively more detailed examples that build on each other and present various SDK concepts
    * [Mobile Gaming Examples](https://beam.apache.org/get-started/mobile-gaming-example/): examples that demonstrate more complex functionality than the WordCount examples.
