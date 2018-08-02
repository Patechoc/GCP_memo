# Serverless Data Analysis with Dataflow


* [Definitions](#definitions)
* [A Simple Dataflow Pipeline (lab 1 in Python)](#a-simple-dataflow-pipeline-lab-1-in-python)
   * [Preparation](#preparation)
      * [Clone data in Cloud Shell](#clone-data-in-cloud-shell)
      * [Create a bucket](#create-a-bucket)
      * [Enable Dataflow](#enable-dataflow)
   * [Open Dataflow project](#open-dataflow-project)
   * [Pipeline filtering](#pipeline-filtering)
   * [Execute the pipeline locally](#execute-the-pipeline-locally)
   * [Execute the pipeline on the cloud](#execute-the-pipeline-on-the-cloud)
* [GroupBy and Combine](#groupby-and-combine)
* [MapReduce in Dataflow (lab 2 in Python)](#mapreduce-in-dataflow-lab-2-in-python)
   * [Task 1. Review Preparations](#task-1-review-preparations)
   * [Task 2. Identify Map and Reduce operations](#task-2-identify-map-and-reduce-operations)
   * [Task 3. Execute the pipeline](#task-3-execute-the-pipeline)
      * [Run the pipeline locally](#run-the-pipeline-locally)
   * [Task 4. Use command line parameters](#task-4-use-command-line-parameters)
* [Side Inputs (lab 3  in Python)](#side-inputs-lab-3--in-python)
   * [Task 1. Preparation](#task-1-preparation)
   * [Task 2. Try using BigQuery query](#task-2-try-using-bigquery-query)
   * [Task 3. Explore the pipeline code](#task-3-explore-the-pipeline-code)
   * [Task 4. Execute the pipeline](#task-4-execute-the-pipeline)
      * [Execute the pipeline locally](#execute-the-pipeline-locally-1)
      * [Execute the pipeline on the cloud](#execute-the-pipeline-on-the-cloud-1)



## Definitions

Dataflow is ...

* Apache Beam on GCP
* an elastic data processing pipeline in the cloud

For examples, you can use it to:

1. read from BigQuery
2. write to Cloud Storage

It defines pipelines that are series of steps (called "transforms"). Each transform is auto-scaled to different machines.

Dataflow can take in:

* historical batch data (bounded sources)
* streaming data (unbounded data)

Apache Beam is an opensource API which can also be used elsewhere (e.g. Spark), but Dataflow is its implementation on Google Cloud. The pipelines are executed by a runner (here Dataflow), an execution framework for this code we have written (either Python or Java).

Each pipeline is a directed graph, it has a "source" and a "sink" and "transforms" in between.

In Python. the `|` (pipe operator) is overloaded to mean `.apply()`.

Every transform is applied to and outputs to a `PCollection` (a "parallel" collection). Each `PCollection` has a given type (String, Integer, BigQuery TableRow, ...).



## A Simple Dataflow Pipeline (lab 1 in Python)

In this lab, you learn how to write a simple Dataflow pipeline and run it both locally and on the cloud.

* Setup a Python Dataflow project using Apache Beam
* Write a simple pipeline in Python
* Execute the query on the local machine
* Execute the query on the cloud


### Preparation

For this lab you will need the [training-data-analyst files](https://github.com/GoogleCloudPlatform/training-data-analyst/tree/master/courses/data_analysis/lab2/python) and a Cloud Storage bucket:

#### Clone data in Cloud Shell

* Verify that the repository files are in Cloud Shell and if not, clone it. Return to the browser tab containing the Cloud Shell code editor (![](https://run-qwiklab-website-prod.s3.amazonaws.com/instructions/documents/51276/original/img/20a5c832dd0ecde9.png)). Click on File > Refresh in the left navigator panel. You should see the training-data-analyst directory.

```shell
cd ~
git clone https://github.com/GoogleCloudPlatform/training-data-analyst
```

#### Create a bucket

Verify that you have a Cloud Storage bucket. If you don't you can follow these instructions to create a bucket:

* In the Console, on the Products & services menu, click Home
* Select and copy the Project ID. For simplicity you will use the Qwiklabs Project ID, which is already globally unique, as the bucket name.
* In the Console, on the Products & services menu () click Storage > Browser. Click Create Bucket.
* Specify the following, and leave the remaining settings as their defaults.
* Click Create.
* Record the name of your bucket. You will need it in subsequent tasks.
* In Cloud Shell enter the following to create an environment variable named "BUCKET" and verify that it exists with the echo command.

```shell
BUCKET="<your unique bucket name (Project ID)>"
echo $BUCKET
```

You can use $BUCKET in Cloud Shell commands. And if you need to enter the bucket name <your-bucket> in a text field in Console, you can quickly retrieve the name with "echo $BUCKET".

#### Enable Dataflow

Verify that Google Cloud Dataflow API is enabled for this project:

* Return to the browser tab for Console. In the top search bar, enter Google Dataflow API. This will take you to the page, Products & Services > APIs & Services > Dashboard > Google Dataflow API. It will either show a status information or it will give you the option to Enable the API.
* If necessary, Enable the API.

### Open Dataflow project

The goal of this lab is to become familiar with the structure of a Dataflow project and learn how to execute a Dataflow pipeline. You will need to update some files to install Apache Beam. Apache Beam is an open source platform for executing data processing workflows.

From Cloud Shell:

```shell
##  navigate to the directory for this lab
cd ~/training-data-analyst/courses/data_analysis/lab2/python


## install the necessary dependencies
sudo ./install_packages.sh
```

Use File > Refresh in Cloud Shell editor to view the local copy of the repository.

> If at any time during the DataFlow labs you are logged out of Cloud Shell due to inactivity, when you login the in-memory elements of Apache Beam will be lost. So you will need to reissue these commands before proceeding.



### Pipeline filtering

```python
#!/usr/bin/env python

"""
Copyright Google Inc. 2016
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at
http://www.apache.org/licenses/LICENSE-2.0
Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
"""

import apache_beam as beam
import re
import sys

def my_grep(line, term):
   if re.match( r'^' + re.escape(term), line):
      yield line

if __name__ == '__main__':
   p = beam.Pipeline(argv=sys.argv)
   input = '../javahelp/src/main/java/com/google/cloud/training/dataanalyst/javahelp/*.java'
   output_prefix = '/tmp/output'
   searchTerm = 'import'

   # find all lines that contain the searchTerm
   (p
      | 'GetJava' >> beam.io.ReadFromText(input)
      | 'Grep' >> beam.FlatMap(lambda line: my_grep(line, searchTerm) )
      | 'write' >> beam.io.WriteToText(output_prefix)
   )

   p.run().wait_until_finish()
```


### Execute the pipeline locally


```python
cd ~/training-data-analyst/courses/data_analysis/lab2/python
python grep.py
```

The output file will be output.txt. If the output is large enough, it will be sharded into separate parts with names like: output-00000-of-00001. If necessary, you can locate the correct file by examining the file's time.

```shell
google764616_student@cloudshell:~/training-data-analyst/courses/data_analysis/lab2/python$ cat /tmp/output-00000-of-00001
import java.time.Instant;
...
import org.apache.beam.sdk.options.PipelineOptionsFactory;
import org.apache.beam.sdk.transforms.DoFn;
import org.apache.beam.sdk.transforms.ParDo;
google764616_student@cloudshell:~/training-data-analyst/courses/data_analysis/lab2/python$
```



### Execute the pipeline on the cloud

* Copy some Java files to the cloud.

```shell
gsutil cp ../javahelp/src/main/java/com/google/cloud/training/dataanalyst/javahelp/*.java gs://$BUCKET/javahelp
```

* Edit the Dataflow pipeline in `grepc.py`. In the Cloud Shell code editor (![](https://run-qwiklab-website-prod.s3.amazonaws.com/instructions/documents/51276/original/img/20a5c832dd0ecde9.png)) navigate to the directory `/training-data-analyst/courses/data_analysis/lab2/python` and edit the file `grepc.py`. Replace PROJECT and BUCKET with your Project ID and Bucket name. Here are easy ways to retrieve the values:

```shell
echo $DEVSHELL_PROJECT_ID
echo $BUCKET
```

Here is `grepc.py`:

```python
#!/usr/bin/env python
"""
Copyright Google Inc. 2016
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at
http://www.apache.org/licenses/LICENSE-2.0
Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
"""
import apache_beam as beam
import re
def my_grep(line, term):
   if re.match( r'^' + re.escape(term), line):
      yield line
PROJECT='qwiklabs-gcp-e50b90da6cc1012e'
BUCKET='qwiklabs-gcp-e50b90da6cc1012e'
def run():
   argv = [
      '--project={0}'.format(PROJECT),
      '--job_name=examplejob2',
      '--save_main_session',
      '--staging_location=gs://{0}/staging/'.format(BUCKET),
      '--temp_location=gs://{0}/staging/'.format(BUCKET),
      '--runner=DataflowRunner'
   ]

   p = beam.Pipeline(argv=argv)
   input = 'gs://{0}/javahelp/*.java'.format(BUCKET)
   output_prefix = 'gs://{0}/javahelp/output'.format(BUCKET)
   searchTerm = 'import'

   # find all lines that contain the searchTerm
   (p
      | 'GetJava' >> beam.io.ReadFromText(input)
      | 'Grep' >> beam.FlatMap(lambda line: my_grep(line, searchTerm) )
      | 'write' >> beam.io.WriteToText(output_prefix)
   )

   p.run()
if __name__ == '__main__':
```



* Submit the Dataflow job to the cloud:

```shell
google764616_student@cloudshell:~/training-data-analyst/courses/data_analysis/lab2/python (qwiklabs-gcp-e50b90da6cc1012e)$ python grepc.py
/usr/local/lib/python2.7/dist-packages/apache_beam/io/gcp/gcsio.py:176: DeprecationWarning: object() takes no parameters
  super(GcsIO, cls).__new__(cls, storage_client))
Collecting apache-beam==2.5.0
  Downloading https://files.pythonhosted.org/packages/c6/96/56469c57cb043f36bfdd3786c463fbaeade1e8fcf0593ec7bc7f99e56d38/apache-beam-2.5.0.zip (2.3MB)
    100% |████████████████████████████████| 2.3MB 6.5MB/s
  Saved /tmp/tmpqahKwR/apache-beam-2.5.0.zip
Successfully downloaded apache-beam
Collecting apache-beam==2.5.0
  Downloading https://files.pythonhosted.org/packages/ff/10/a59ba412f71fb65412ec7a322de6331e19ec8e75ca45eba7a0708daae31a/apache_beam-2.5.0-cp27-cp27mu-manylinux1_x86_64.whl (2.2MB)
    100% |████████████████████████████████| 2.2MB 6.4MB/s
  Saved /tmp/tmpqahKwR/apache_beam-2.5.0-cp27-cp27mu-manylinux1_x86_64.whl
Successfully downloaded apache-beam
google764616_student@cloudshell:~/training-data-analyst/courses/data_analysis/lab2/python (qwiklabs-gcp-e50b90da6cc1012e)$
```

Because this is such a small job, running on the cloud will take significantly longer than running it locally (on the order of 2-3 minutes).

* Return to the browser tab for Console. On the Products & services menu () click Dataflow and click on your job to monitor progress.

![monitor progress](https://run-qwiklab-website-prod.s3.amazonaws.com/instructions/documents/51251/original/img/19dfd2243d1e0ece.png)


* Wait for the job status to turn to Succeeded. At this point, your Cloud Shell will display a command-line prompt.

* Examine the output in the Cloud Storage bucket. On the Products & services menu. click Storage > Browser and click on your bucket. Click the javahelp directory.

This job will generate the file `output.txt`. If the file is large enough it will be sharded into multiple parts with names like: `output-0000x-of-000y`. You can identify the most recent file by name or by the **Last modified** field. Click on the file to view it.

Alternatively, you could download the file in Cloud Shell and view it:

```shell
$ gsutil  cp gs://qwiklabs-gcp-e50b90da6cc1012e/javahelp/output-0000*
 .
Copying gs://qwiklabs-gcp-e50b90da6cc1012e/javahelp/output-00000-of-00004...
Copying gs://qwiklabs-gcp-e50b90da6cc1012e/javahelp/output-00001-of-00004...
Copying gs://qwiklabs-gcp-e50b90da6cc1012e/javahelp/output-00002-of-00004...
Copying gs://qwiklabs-gcp-e50b90da6cc1012e/javahelp/output-00003-of-00004...
\ [4 files][  2.5 KiB/  2.5 KiB]
Operation completed over 4 objects/2.5 KiB.
$ cat output-00000-of-00004
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
import java.util.Map;
import com.google.api.services.bigquery.model.TableRow;
import org.apache.beam.sdk.Pipeline;
import org.apache.beam.sdk.io.gcp.bigquery.BigQueryIO;
import org.apache.beam.sdk.io.TextIO;
import org.apache.beam.sdk.options.Default;
import org.apache.beam.sdk.options.Description;
import org.apache.beam.sdk.options.PipelineOptions;
import org.apache.beam.sdk.options.PipelineOptionsFactory;
import org.apache.beam.sdk.transforms.DoFn;
import org.apache.beam.sdk.transforms.ParDo;
import org.apache.beam.sdk.transforms.Sum;
import org.apache.beam.sdk.transforms.Top;
import org.apache.beam.sdk.transforms.View;
import org.apache.beam.sdk.values.KV;
import org.apache.beam.sdk.values.PCollection;
import org.apache.beam.sdk.values.PCollectionView;
```




## GroupBy and Combine

In JAVA: `Combine.globally()` and `Combine.perKey()`. You can use 

In Python: `beam.GroupByKey()`,  `Combine.globally(sum)` and `Combine.perKey(sum)`


```python
cityAndZipcodes = p | beam.Map(lambda fields : (fields[0], fields[3]))

grouped = cityAndZipcodes | beam.GroupByKey()
```


```python
totalAmount = SalesAmounts | Combine.globally(sum)
```

```python
totalSalesPerPerson = salesRecords | Combine.perKey(sum)
```


> NB1: Compared to JAVA, remember the different types of your PCollections in Python (doubles, tuples, ...).

> NB2: Prefer `Combine` over `GroupByKey`  (if the function applied already exists, e.g. `sum`, `count`, `min`, `mean`, ...). Only use `GroupByKey` if you process a specific homemade function.

```java
collection.apply(Count.perKey())
```

is faster than


```java
collection
	.apply(GroupByKey.create())
	.apply(ParDo.of(new DoFn() {
	  void processElement(ProcessContext c) {
	     c.output(KV.of(c.element().getKey(), c.element().getValue().size() ));
	     }
	  }
	 )
	)
```



## MapReduce in Dataflow (lab 2 in Python)

* Identify Map and Reduce operations
* Execute the pipeline
* Use command line parameters


Use the Google Cloud Shell Code Editor to easily create and edit directories and files in the Cloud Shell instance.

* After you open the Google Cloud Shell, click **Launch the code editor** (![](https://run-qwiklab-website-prod.s3.amazonaws.com/instructions/documents/51276/original/img/20a5c832dd0ecde9.png)) to open the Cloud Shell Code Editor.




### Task 1. Review Preparations

These preparations should already be have been done:

* [Create Cloud Storage bucket](#create-a-bucket)
* [Clone github repository to Cloud Shell](#clone-data-in-cloud-shell)
* [Upgrade packages and install Apache Beam](#open-dataflow-project)

### Task 2. Identify Map and Reduce operations

In the Cloud Shell code editor navigate to the directory `/training-data-analyst/courses/data_analysis/lab2/javahelp/src/main/java/com/google/cloud/training/dataanalyst/javahelp/` and view the file [`IsPopular.java`](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/courses/data_analysis/lab2/javahelp/src/main/java/com/google/cloud/training/dataanalyst/javahelp/IsPopular.java) in the Cloud Shell editor. **Do not make any changes to the code.**


```shell
git clone https://github.com/GoogleCloudPlatform/training-data-analyst
sudo ./install_packages.sh

cd ~/training-data-analyst/courses/data_analysis/lab2/javahelp/src/main/java/com/google/cloud/training/dataanalyst/javahelp/
nano IsPopular.java
```




### Task 3. Execute the pipeline

#### Run the pipeline locally

Check and run the script: [`is_popular.py`](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/courses/data_analysis/lab2/python/is_popular.py)


```shell
cd ~/training-data-analyst/courses/data_analysis/lab2/python
./is_popular.py
```


That's we are mostly processing:

```python
   # find most used packages
   (p
      | 'GetJava' >> beam.io.ReadFromText(input)
      | 'GetImports' >> beam.FlatMap(lambda line: startsWith(line, keyword))
      | 'PackageUse' >> beam.FlatMap(lambda line: packageUse(line, keyword))
      | 'TotalUse' >> beam.CombinePerKey(sum)
      | 'Top_5' >> beam.transforms.combiners.Top.Of(5, by_value)
      | 'write' >> beam.io.WriteToText(output_prefix)
   )
```


Identify the output file. It should be output<suffix> and could be a sharded file.


```shell
ls -al /tmp
total 92
drwxrwxrwt 1 root                 root                  4096 Aug  2 13:36 .
drwxr-xr-x 1 root                 root                  4096 Aug  2 13:24 ..
-rw-r--r-- 1 root                 root                   751 Jul 31 20:47 b63773568.patch
drwxr-xr-x 3 root                 root                  4096 Jul 31 20:51 dotnet-installer
drwxr-xr-x 2 root                 root                  4096 Jul 31 20:50 hsperfdata_root
-rw-r--r-- 1 root                 root                 15895 Jul 31 21:02 metadata.go
drwxr-xr-x 2 root                 root                  4096 Jul 31 20:55 npm-427-a5ef501b
-rw-r--r-- 1 google769005_student google769005_student   133 Aug  2 13:36 output-00000-of-00001
-rw------- 1 root                 root                  4502 Jul 31 21:00 tmpBGXKUk
-rw------- 1 root                 root                 31618 Jul 31 21:01 tmpgvbjXc
-rw------- 1 root                 root                     0 Aug  2 13:24 tmp.mdGla6AurF
drwx------ 3 google769005_student google769005_student  4096 Aug  2 13:24 tmp.YFr6lS45xf
drwx------ 2 google769005_student google769005_student  4096 Aug  2 13:24 tmux-1000
```

Examine the output file, replacing '-*' with the appropriate suffix.

Here is the top 5 of the JAVA packages used:

```shell
$ cat /tmp/output-00000-of-00001
[(u'org', 45), (u'org.apache.beam', 44), (u'org.apache', 44), (u'org.apache.beam.sdk', 43), (u'org.apache.beam.sdk.transforms', 16)]
```



### Task 4. Use command line parameters


Change the output prefix from the default value:


```shell
./is_popular.py --output_prefix=/tmp/myoutput

ls -al /tmp
total 96
drwxrwxrwt 1 root                 root                  4096 Aug  2 13:42 .
drwxr-xr-x 1 root                 root                  4096 Aug  2 13:24 ..
-rw-r--r-- 1 root                 root                   751 Jul 31 20:47 b63773568.patch
...
-rw-r--r-- 1 google769005_student google769005_student   133 Aug  2 13:42 myoutput-00000-of-00001
...
```

## Side Inputs (lab 3  in Python)

idea: As a programmer, which opensource project shoud you contribute to?

In this lab series, you learn how to load data into BigQuery and run complex queries. Next, you will execute a Dataflow pipeline that can carry out Map and Reduce operations, use side inputs and stream into BigQuery.

* Get data from BigQuery (how popular packages in Github data, and look for "fix me" and "todo") into Dataflow
* Use the output of a pipeline as a side-input to another pipeline (in a `apply()` )



### Task 1. Preparation

* clone the code

```shell
cd ~
git clone https://github.com/GoogleCloudPlatform/training-data-analyst
```

* Verify that you have a Cloud Storage bucket or create one
* Verify environment variable for your Project ID (`echo $DEVSHELL_PROJECT_ID`)
* Verify that Google Cloud Dataflow API is enabled for this project or enable the API
* Verify that Apache Beam is installed on Cloud Shell

```shell
cd ~/training-data-analyst/courses/data_analysis/lab2/python
sudo ./install_packages.sh
```



### Task 2. Try using BigQuery query

Open the BigQuery web UI from the [Console](http://console.cloud.google.com/).

Compose a new query:

> Before running the query, click on **Options**. Verify that Use **Legacy SQL** is checked.

```sql
SELECT
  content
FROM
  [fh-bigquery:github_extracts.contents_java_2016]
LIMIT
  10
```

**What is being returned?**

The BigQuery table fh-bigquery:github_extracts.contents_java_2016 contains the content (and some metadata) of all the Java files present in github in 2016.

To find out how many Java files this table has, type the following query and click Run Query:

```sql
SELECT
  COUNT(*)
FROM
  [fh-bigquery:github_extracts.contents_java_2016]
```

you get 2196247!

2196247 files better processed in the cloud than locally :)



### Task 3. Explore the pipeline code

Get the pipeline code from Cloud Storage if it's still unavailable from github:

```shell
cd ~/training-data-analyst/courses/data_analysis/lab2/python
gsutil cp gs://cloud-training/gcpdei/JavaProjectsThatNeedHelp.py .
```

Refer to this diagram as you read the code. The pipeline looks like this:

![pipeline](https://run-qwiklab-website-prod.s3.amazonaws.com/instructions/documents/51253/original/img/510672e8b03602d.png)

> The Java version of this program is slightly different from the Python version. The Java SDK supports `AsMap` and the Python SDK doesn't. It supports `AsDict` instead. In Java, the `PCollection` is converted into a **View** as a preparatory step before it is used. In Python the `PCollection` conversion occurs in the step where it is used.

Answer the following questions:

* **Looking at the class documentation at the very top, what is the purpose of this pipeline?**
  "This is a dataflow pipeline that demonstrates Python use of side inputs. The pipeline finds Java packages
on Github that are (a) popular and (b) need help. Popularity is use of the package in a lot of other
projects, and is determined by counting the number of times the package appears in import statements.
Needing help is determined by counting the number of times the package contains the words FIXME or TODO
in its source."


* **Where does the content come from?**
  content comes from Github JAVA files from 2016 (fh-bigquery:github_extracts.contents_java_2016)

* **What does the left side of the pipeline do?**
  Looking for "todo" and "fix me" in the code.

* **What does the right side of the pipeline do?**
  Looking for most popular packages imported

* **What does ToLines do? (Hint: look at the content field of the BigQuery result)**
  It splits the content of a JAVA file into lines


* **Why is the result of ReadFromBQ stored in a named PCollection instead of being directly passed to another step?**
  Maybe to check how much time this step takes!?

* **What are the two actions carried out on the PCollection generated from ReadFromBQ?**
  It performs a query to BigQuery and converts it to a `PCollection` in Dataflow using Apache Beam

* **If a file has 3 FIXMEs and 2 TODOs in its content (on different lines), how many calls for help are associated with it?**
  5 

* **If a file is in the package com.google.devtools.build, what are the packages that it is associated with?**
  packages: 'com', 'com.google', 'com.google.devtools','com. google.devtools.build'

* **popular_packages and help_packages are both named PCollections and both used in the Scores (side inputs) step of the pipeline. Which one is the main input and which is the side input?**
  `results = popular_packages | 'SideInputs' >> beam.Map(lambda element, the_dict: compositeScore(element,the_dict), beam.pvalue.AsDict(help_packages))`

  `help_packages` is a side-input to `popular_packages` 
  

* **What is the method used in the Scores step?**
  `compositeScore()` computes the score as the product of the logarithm of its popularity and demand of help.

* **What Python data type is the side input converted into in the Scores step?**
  It is converted as a dictionary (`beam.pvalue.AsDict(help_packages)`)




### Task 4. Execute the pipeline


Change into the directory. The program requires BUCKET and PROJECT values and choosing whether to run the pipeline locally using `--DirectRunner` or on the cloud using `--DataFlowRunner`.

```shell
cd ~/training-data-analyst/courses/data_analysis/lab2/python
echo $BUCKET
echo $DEVSHELL_PROJECT_ID
```

Make sure these variables are not empty!


#### Execute the pipeline locally

By typing the following into Cloud Shell:

```shell
python JavaProjectsThatNeedHelp.py --bucket $BUCKET --project $DEVSHELL_PROJECT_ID --DirectRunner
```

Once the pipeline has finished executing, On the **Products & services** menu (![menu](https://lh3.googleusercontent.com/aM9cftz4R80-cn1YoOzXUEL57hhkHsV2Dd-nGBdt_qjfOsFqXz0MNUDOkD99UQaYTefEbJldtJEjgSryvQcxtqQv39bQNGIUUOhUDp5XEDydbmbb27O5giNs7nYjavcIwL6Fi6-i)) click **Storage > Browser** and click on your bucket. You will find the results in the **javahelp** folder. Click on the **Result** object to examine the output.


Here is what you see while opening `Results.csv`:


```csv
[(u'org', 28.068221258521774), (u'com', 32.07294420077314), (u'net', 8.50255292188352), (u'edu', 11.836868328431866), (u'org.apache', 9.074090434545194), (u'fr', 9.52941938400277), (u'com.google', 6.318389861893019), (u'io', 9.906351880740292), (u'fr.lip6', 7.545241083952861), (u'fr.lip6.move.pnml', 7.545241083952861), (u'fr.lip6.move', 7.545241083952861), (u'br', 3.6993525784040227), (u'edu.toronto.cs', 4.346717950866613), (u'edu.toronto', 4.346717950866613), (u'edu.toronto.cs.se', 4.346717950866613), (u'com.github', 4.358438662428687), (u'edu.toronto.cs.se.modelepedia', 2.8993008128149693), (u'com.google.common', 2.6343572136354596), (u'fr.lip6.move.pnml.pthlpng', 2.13780080239616), (u'ru', 1.3488021334406732), (u'me', 2.13780080239616), (u'com.example', 2.8837256197384322), (u'com.google.common.collect', 1.7681482684484517), (u'fr.lip6.move.pnml.hlpn', 2.8837256197384322), (u'com.sina', 4.5598809699382725), (u'edu.rpi.tw', 1.523000020837618), (u'io.tilt', 3.046000041675236), (u'io.tilt.minka', 3.046000041675236), (u'se', 2.6976042668813465), (u'edu.rpi.tw.linkipedia.search', 1.523000020837618), (u'edu.rpi.tw.linkipedia', 1.523000020837618), (u'edu.rpi', 1.523000020837618), (u'fr.lip6.move.pnml.pthlpng.terms', 1.523000020837618), (u'ru.agentlab', 0.761500010418809), (u'alexclin', 2.7299489816502), (u'net.sharkfw.knowledgeBase', 1.206948960812582), (u'com.fuav.android', 0.761500010418809), (u'com.facebook', 0.761500010418809), (u'com.fuav', 0.761500010418809), (u'ru.agentlab.maia', 0.761500010418809), (u'net.sharkfw', 1.206948960812582), (u'alexclin.httplite', 2.7299489816502), (u'org.apache.logging', 0.761500010418809), (u'org.apache.logging.log4j', 0.761500010418809), (u'localsearch.domainspecific', 2.8379833895451925), (u'localsearch.domainspecific.vehiclerouting', 2.8379833895451925), (u'localsearch', 2.8379833895451925), (u'localsearch.domainspecific.vehiclerouting.vrp', 2.8379833895451925), (u'utils', 0.4804530139182014), (u'context', 1.7224060382552118), (u'org.fortiss.smg', 1.2419530243370103), (u'org.chromium', 1.7778874154035074), (u'com.fuav.android.core', 0.4804530139182014), (u'org.usfirst', 0.761500010418809), (u'context.arch', 1.7224060382552118), (u'org.fortiss', 1.2419530243370103)]
```

#### Execute the pipeline on the cloud

By typing the following into Cloud Shell:

```shell
python JavaProjectsThatNeedHelp.py --bucket $BUCKET --project $DEVSHELL_PROJECT_ID --DataFlowRunner
```

Return to the browser tab for Console. On the **Products & services** menu (![menu](https://lh3.googleusercontent.com/aM9cftz4R80-cn1YoOzXUEL57hhkHsV2Dd-nGBdt_qjfOsFqXz0MNUDOkD99UQaYTefEbJldtJEjgSryvQcxtqQv39bQNGIUUOhUDp5XEDydbmbb27O5giNs7nYjavcIwL6Fi6-i)) click **Dataflow** and click on your job to monitor progress.

Once the pipeline has finished executing, On the **Products & services** menu (![menu](https://lh3.googleusercontent.com/aM9cftz4R80-cn1YoOzXUEL57hhkHsV2Dd-nGBdt_qjfOsFqXz0MNUDOkD99UQaYTefEbJldtJEjgSryvQcxtqQv39bQNGIUUOhUDp5XEDydbmbb27O5giNs7nYjavcIwL6Fi6-i)) click **Storage > Browser** and click on your bucket. You will find the results in the **javahelp** folder. Click on the **Result** object to examine the output.