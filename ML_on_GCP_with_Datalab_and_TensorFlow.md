# Memo for TensorFlow on Google Cloud Platform


## Recommended extra courses

* [Machine learning with TensorFlow on GCP](https://www.coursera.org/specializations/machine-learning-tensorflow-gcp) (5 courses, 7+ weeks)
* Advanced machine learning with TensorFlow on GCP: ??
* https://ai.google/education/
* https://developers.google.com/machine-learning/crash-course/

## Definition

## Playing with ML

## Effective ML model

## What makes a good dataset

### Error metrics

### Accuracy

### Precision and Recall

### Creating ML datasets

## Lab 1: Explore dataset, create ML datasets, create benchmark with taxi fares in NYC

In this lab you will perform the following tasks:

* Sample the dataset and create training, validation, and testing datasets for local development of TensorFlow models
* Create a benchmark to evaluate the performance of ML

### Task 1. Launch Cloud Datalab

* Return to the browser tab containing Cloud Shell: https://console.cloud.google.com
* Open Cloud Shell Code Editor: https://console.cloud.google.com/cloudshell/editor
* Select a zone that is near your location. Enter the following into Cloud Shell to see a list of available zones:

```shell
$ gcloud compute zones list
NAME                       REGION                   STATUS  NEXT_MAINTENANCE  TURNDOWN_DATE
europe-west4-a             europe-west4             UP
europe-west4-b             europe-west4             UP
europe-west4-c             europe-west4             UP
europe-west1-b             europe-west1             UP
europe-west1-d             europe-west1             UP
europe-west1-c             europe-west1             UP
europe-west3-b             europe-west3             UP
europe-west3-c             europe-west3             UP
europe-west3-a             europe-west3             UP
europe-west2-c             europe-west2             UP
europe-west2-b             europe-west2             UP
europe-west2-a             europe-west2             UP
europe-north1-a            europe-north1            UP
europe-north1-b            europe-north1            UP
europe-north1-c            europe-north1            UP
northamerica-northeast1-a  northamerica-northeast1  UP
...
us-west2-c                 us-west2                 UP
google803005_student@qwiklabs-gcp-7da27dc7d07d9a01:~$
```


> Please pick a zone in a geographically close region from the following: us-east1, us-central1, asia-east1, europe-west1. These are the regions that currently support Cloud ML Engine jobs. Please verify [here (cloud_compute_regions)](https://cloud.google.com/ml-engine/docs/tensorflow/environment-overview#cloud_compute_regions) since this list may have changed after this lab was last updated. For example, if you are in the US, you may choose us-east1-c as your zone.

* Start Datalab in your region:

```shell
datalab create dataengvm --zone europe-north1-a
```

Datalab setup will prompt you to continue. Enter 'Y'.

Datalab setup will ask you for a passphrase. You can press Enter twice.

Datalab will take about five minutes to start. Datalab is ready when you see a message prompting you to do a "Web Preview".

> The connection to your Datalab instance remains open for as long as the datalab command is active. If the cloud shell used for running the datalab command is closed or interrupted, the connection to your Cloud Datalab VM will terminate.

### Task 2. Clone repo into Cloud Datalab

* Click on the Web Preview icon on the top-right corner of the Cloud Shell ribbon.

![icon](https://run-qwiklab-website-prod.s3.amazonaws.com/instructions/documents/46298/original/img/81a5d766feed3a8e.png)

* Click on **Change Port**.
* In the **Change Preview Port** dialog, in the **Port Number** box, enter **8081**.
* Click **Change and Preview**.
* In Cloud Datalab home page (browser), open a new notebook 


In the new notebook, enter the following commands in the cell, and click on Run (on the top navigation bar) to run the commands:

```python
%bash
git clone https://github.com/GoogleCloudPlatform/training-data-analyst
cd training-data-analyst
```

Confirm that you have cloned the repo by going back to Datalab browser, and ensure you see the **training-data-analyst** directory.


### Task 3: Run the lab in the notebook


In Cloud Datalab, click on the Home icon, and then navigate to **datalab/training-data-analyst/courses/machine_learning/datasets/** or check [Github to see its content](https://github.com/GoogleCloudPlatform/training-data-analyst/tree/master/courses/machine_learning/datasets).

Open [**create_datasets.ipynb**](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/courses/machine_learning/datasets/create_datasets.ipynb). Read the narrative and execute each cell in turn.

#### Explore the data

* Extract sample data from BigQuery (~1 millions rows out of 1 bllions available). Write out a .csv file from BigQuery to make it easy to work with. (reduces cost also and more pratical for exploration)
* visualize correlation between fares and trip distances, and figure out what parameters to use.
* Clean up data by removing errors & outliers ("a lot of invalid data that is being coded as zero distance and some fare amounts that are definitely illegitimate")
* check the the distribution of values within the columns (with `df.describe()`) >> "The min, max of longitude look strange."
* visualize the start and end of a few trips ("As you'd expect, rides that involve a toll are longer than the typical ride")

#### Quality control and other preprocessing

We need to some clean-up of the data:

* New York city longitudes are around -74 and latitudes are around 41.
* We shouldn't have zero passengers.
* Clean up the total_amount column to reflect only fare_amount and tolls_amount, and then remove those two columns. Before the ride starts, we'll know the pickup and dropoff locations, but not the trip distance (that depends on the route taken), so remove it from the ML dataset
* Discard the timestamp

You could do preprocessing in BigQuery, similar to how the zero-distance rides were removed, but you can also do this in Python.

In production, you will have to carry out the same preprocessing on the real-time input data.

This sort of preprocessing of input data is quite common in ML, especially if the quality-control is dynamic.


Using `df.describe()`  again, you see that the quality control has removed about 300 rows (11400 - 11101) or about 3% of the data. This seems reasonable.

#### Creating the ML datasets

* Split the QCed data randomly (`df.sample(frac=1)`) into **training**, **validation** and **test** sets.


```python
shuffled = tripsqc.sample(frac=1)
trainsize = int(len(shuffled['fare_amount']) * 0.70)
validsize = int(len(shuffled['fare_amount']) * 0.15)

df_train = shuffled.iloc[:trainsize, :]
df_valid = shuffled.iloc[trainsize:(trainsize+validsize), :]
df_test = shuffled.iloc[(trainsize+validsize):, :]
```

* Use `describe()` to check each resulting dataframe.

* Write out the three dataframes to appropriately named csv files. We can use these csv files for local training (recall that these files represent only 1/100,000 of the full dataset) until we get to point of using Dataflow and Cloud ML. 

```python
def to_csv(df, filename):
  outdf = df.copy(deep=False)
  outdf.loc[:, 'key'] = np.arange(0, len(outdf)) # rownumber as key
  # reorder columns so that target is first column
  cols = outdf.columns.tolist()
  cols.remove('fare_amount')
  cols.insert(0, 'fare_amount')
  print (cols)  # new order of columns
  outdf = outdf[cols]
  outdf.to_csv(filename, header=False, index_label=False, index=False)

to_csv(df_train, 'taxi-train.csv')
to_csv(df_valid, 'taxi-valid.csv')
to_csv(df_test, 'taxi-test.csv')
```


#### Benchmark

Before we start building complex ML models, it is a good idea to come up with a very simple model and use that as a benchmark.

Build a benchmark to find out if you do something useful, better or worse than heuristic when using ML!

This model is going to simply divide the mean `fare_amount` by the mean `trip_distance` to come up with a rate and use that to predict. Let's compute the RMSE of such a model.

```python
import datalab.bigquery as bq
import pandas as pd
import numpy as np
import shutil

def distance_between(lat1, lon1, lat2, lon2):
  # haversine formula to compute distance "as the crow flies".  Taxis can't fly of course.
  dist = np.degrees(np.arccos(np.minimum(1,np.sin(np.radians(lat1)) * np.sin(np.radians(lat2)) + np.cos(np.radians(lat1)) * np.cos(np.radians(lat2)) * np.cos(np.radians(lon2 - lon1))))) * 60 * 1.515 * 1.609344
  return dist

def estimate_distance(df):
  return distance_between(df['pickuplat'], df['pickuplon'], df['dropofflat'], df['dropofflon'])

def compute_rmse(actual, predicted):
  return np.sqrt(np.mean((actual-predicted)**2))

def print_rmse(df, rate, name):
  print ("{1} RMSE = {0}".format(compute_rmse(df['fare_amount'], rate*estimate_distance(df)), name))

FEATURES = ['pickuplon','pickuplat','dropofflon','dropofflat','passengers']
TARGET = 'fare_amount'
columns = list([TARGET])
columns.extend(FEATURES) # in CSV, target is the first column, after the features
columns.append('key')
df_train = pd.read_csv('taxi-train.csv', header=None, names=columns)
df_valid = pd.read_csv('taxi-valid.csv', header=None, names=columns)
df_test = pd.read_csv('taxi-test.csv', header=None, names=columns)
rate = df_train['fare_amount'].mean() / estimate_distance(df_train).mean()
print ("Rate = ${0}/km".format(rate))
print_rmse(df_train, rate, 'Train')
print_rmse(df_valid, rate, 'Valid') 
print_rmse(df_test, rate, 'Test')
```


The RMSE depends on the dataset, and for comparison, we have to **evaluate on the same dataset each time**.

The simple distance-based rule gives us a RMSE of $8.03. We have to beat this with our next models, of course, but you will find that simple rules of thumb like this can be surprisingly difficult to beat.

Let's be ambitious, though, and make our goal to build ML models that have a RMSE of less than $6 on the test set.

`Lab 1a completed`






## Other references


* [Angeliqa's notes](https://docs.google.com/document/d/1_G6F5qmjcklNOK8KBkUBD5gDHMLXrNjCX6yIBuR8iEA/edit?pli=1)
* [GCP Labs and demos for the course](https://github.com/GoogleCloudPlatform/training-data-analyst)
* [Cloud Datalab resources on Github](https://github.com/googledatalab/notebooks)