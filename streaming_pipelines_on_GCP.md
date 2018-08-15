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

## Content






* "So if you want to look for fraud transactions historically, well that's **batch**,"
* "but if you want to look at it as it happens, that **streaming**."



### GCP Beam/Dataflow

* what
* where
* when
* how ...

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

* Download the traffic simulation dataset ([download_data.sh](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/courses/streaming/publish/download_data.sh)).

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

* Run the [send_sensor_data.py](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/courses/streaming/publish/send_sensor_data.py)

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




```shell

```


----------------------------------------------------------------------------------

## Other references



----------------------------------------------------------------------------------

## Recommended extra courses

