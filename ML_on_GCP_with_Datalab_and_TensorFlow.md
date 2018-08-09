# Memo for TensorFlow on Google Cloud Platform

## Content

* [Definition](#definition)
* [Playing with ML](#playing-with-ml)
* [Effective ML model](#effective-ml-model)
* [What makes a good dataset](#what-makes-a-good-dataset)
   * [Error metrics](#error-metrics)
   * [Accuracy](#accuracy)
   * [Precision and Recall](#precision-and-recall)
   * [Creating ML datasets](#creating-ml-datasets)
* [Lab 1: Explore dataset, create ML datasets, create benchmark with taxi fares in NYC](#lab-1-explore-dataset-create-ml-datasets-create-benchmark-with-taxi-fares-in-nyc)
   * [Task 1. Launch Cloud Datalab](#task-1-launch-cloud-datalab)
   * [Task 2. Clone repo into Cloud Datalab](#task-2-clone-repo-into-cloud-datalab)
   * [Task 3: Run the lab in the notebook](#task-3-run-the-lab-in-the-notebook)
      * [Explore the data](#explore-the-data)
      * [Quality control and other preprocessing](#quality-control-and-other-preprocessing)
      * [Creating the ML datasets](#creating-the-ml-datasets)
      * [Benchmark](#benchmark)
* [TensorFlow](#tensorflow)
   * [Core TensorFlow](#core-tensorflow)
* [Lab 2: Getting Started with TensorFlow](#lab-2-getting-started-with-tensorflow)
   * [Task 1. Launch Cloud Datalab](#task-1-launch-cloud-datalab-1)
   * [Task 2. Clone repo into Cloud Datalab](#task-2-clone-repo-into-cloud-datalab-1)
   * [Task 3: Run the lab in the notebook](#task-3-run-the-lab-in-the-notebook-1)
      * [Getting started with TensorFlow](#getting-started-with-tensorflow)
      * [Placeholder and feed_dict](#placeholder-and-feed_dict)
      * [tf.eager](#tfeager)
* [TensorFlow for ML](#tensorflow-for-ml)
* [Lab 3: Machine Learning using tf.estimator](#lab-3-machine-learning-using-tfestimator)
   * [Task 1. Launch Cloud Datalab](#task-1-launch-cloud-datalab-2)
   * [Task 2. Clone repo into Cloud Datalab](#task-2-clone-repo-into-cloud-datalab-2)
   * [Task 3: Run the lab in the notebook](#task-3-run-the-lab-in-the-notebook-2)
      * [Read data created in the previous lab](#read-data-created-in-the-previous-lab)
      * [Input function to read from Pandas Dataframe into tf.constant](#input-function-to-read-from-pandas-dataframe-into-tfconstant)
      * [Create feature columns for estimator](#create-feature-columns-for-estimator)
      * [Linear Regression with tf.Estimator framework](#linear-regression-with-tfestimator-framework)
      * [Evaluate on the validation data](#evaluate-on-the-validation-data)
      * [Deep Neural Network regression](#deep-neural-network-regression)
      * [Benchmark dataset](#benchmark-dataset)
* [Distributed TensorFlow models](#distributed-tensorflow-models)
   * [Write TensorFlow graphs and Training an evaluation loop](#write-tensorflow-graphs-and-training-an-evaluation-loop)
   * [Monitor ML training with TensorBoard](#monitor-ml-training-with-tensorboard)
* [Other references](#other-references)
* [Recommended extra courses](#recommended-extra-courses)


---

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




## TensorFlow

TensorFlow name: scalar (number), vector (1D array of vector), matrix (2D array), tensor (array of rank 3 or more). A tensor is an N-dimensional array of data.

Your data in TF are tensors. The tensors flow through the graph, i.e. TensorFlow, an open-source high-perfmance library for numerical computaiton that uses directed graphs.

TF can be called at different level of abstractions (working on different hardwares, core in C++, many APIs like the Python API, components for building custom NN models, high-level out-of-the-box API).

**Cloud ML Engine** is the execution environment to free you from managing any infrastructure.

### Core TensorFlow

First step, build a graph:

```python
...
c = tf.add(a,b )
```

Second step, run the graph:

Only evaluated when run through a session (lazy evaluation)

```python
session = tf.Session()
numpy_c = session.run(c, feed_dict=...)
```

TF returns Numpy arrays (C-based efficient numerical library), only when the session evaluate the graph.



## Lab 2: Getting Started with TensorFlow

In this lab you will perform the following tasks:

* Explore the TensorFlow Python API
* Building a graph
* Running a graph
* Feeding values into a graph
* Find area of a triangle using TensorFlow


### Task 1. Launch Cloud Datalab

* Launch [Google Cloud Shell Code Editor](https://console.cloud.google.com/cloudshell/editor)

```shell
datalab create dataengvm --zone europe-north1-a
```

more details here [Task 1. Launch Cloud Datalab](#task-1-launch-cloud-datalab)


### Task 2. Clone repo into Cloud Datalab

```python
%bash
git clone https://github.com/GoogleCloudPlatform/training-data-analyst
cd training-data-analyst
```

### Task 3: Run the lab in the notebook


In Cloud Datalab, click on the Home icon, and then navigate to **datalab/training-data-analyst/courses/machine_learning/tensorflow**
or check [Github to see its content](https://github.com/GoogleCloudPlatform/training-data-analyst/tree/master/courses/machine_learning/tensorflow).

Open [**a_tfstart.ipynb**](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/courses/machine_learning/tensorflow/a_tfstart.ipynb). Read the narrative and execute each cell in turn.

#### Getting started with TensorFlow 

```python
import tensorflow as tf
import numpy as np

print tf.__version__
# 1.8.0

# Adding 2 tensors using numpy, the Python numeric package
a = np.array([5, 3, 8])
b = np.array([3, -1, 2])
c = np.add(a, b)
print c
# [ 8  2 10]


# Step 1: Build the equivalent graph in TensorFlow
a = tf.constant([5, 3, 8])
b = tf.constant([3, -1, 2])
c = tf.add(a, b)
print c
# Tensor("Add:0", shape=(3,), dtype=int32)

# Step 2: Run the graph
with tf.Session() as sess:
  result = sess.run(c)
  print result
# [ 8  2 10]

# Using a feed_dict
a = tf.placeholder(dtype=tf.int32, shape=(None,))  # batchsize x scalar
b = tf.placeholder(dtype=tf.int32, shape=(None,))
c = tf.add(a, b)
with tf.Session() as sess:
  result = sess.run(c, feed_dict={
      a: [3, 4, 5],
      b: [-1, 2, 3]
    })
  print result
# [2 6 8]
```

Look up the available operations at https://www.tensorflow.org/api_docs/python/tf

#### Placeholder and feed_dict

More common is to define **the input to a program** as **a placeholder** and then to feed in the inputs.

The difference between the code below and the code without placeholder is whether the "area" graph is coded up with the input values or whether the "area" graph is coded up with a placeholder through which inputs will be passed in at run-time.

```python
with tf.Session() as sess:
  sides = tf.placeholder(tf.float32, shape=(None, 3))  # batchsize number of triangles, 3 sides
  area = compute_area(sides)
  result = sess.run(area, feed_dict = {
      sides: [
        [5.0, 3.0, 7.1],
        [2.3, 4.1, 4.8]
      ]
    })
  print result
# [6.278497 4.709139]  
```

#### tf.eager

tf.eager allows you to avoid the build-then-run stages. However, most production code will follow the lazy evaluation paradigm because the lazy evaluation paradigm is what allows for multi-device support and distribution.

One thing you could do is to develop using tf.eager and then comment out the eager execution and add in the session management code.

> To run this block, you must first reset the notebook using Reset on the menu bar, then run this block only.

```python
import tensorflow as tf
from tensorflow.contrib.eager.python import tfe

tfe.enable_eager_execution()

def compute_area(sides):
  # slice the input to get the sides
  a = sides[:,0]  # 5.0, 2.3
  b = sides[:,1]  # 3.0, 4.1
  c = sides[:,2]  # 7.1, 4.8
  
  # Heron's formula
  s = (a + b + c) * 0.5   # (a + b) is a short-cut to tf.add(a, b)
  areasq = s * (s - a) * (s - b) * (s - c) # (a * b) is a short-cut to tf.multiply(a, b), not tf.matmul(a, b)
  return tf.sqrt(areasq)

area = compute_area(tf.constant([
      [5.0, 3.0, 7.1],
      [2.3, 4.1, 4.8]
    ]))


print area
```


## TensorFlow for ML

[video](https://www.coursera.org/learn/serverless-machine-learning-gcp/lecture/L9E5T/estimator-api)


```python
import tensorflow as tf

# Define input feature columns
deatcols = [
	 tf.feature_column.numeric_column("sq_footage") ]

# instantiate Linear Regression Model
model = tf.estimator.LinearRegressor(featcols, './model_trained')

# Train
def train_input_fn():
    features = {
    "sq_footage": tf.constant([1000, 2000]) }
    labels =tf.constant([50, 100]) # in thousands
    return features, labels
model.train(train_input_fn, steps=100)

# Predict
def pred_input_fn():
    features = {
    	     "sq_footage": tf.constant([1500, 1800]) }

    return features
out = trained.predict(pred_input_fn)    

```

Other types of Regressors:

* Deep neural network


```python
model = DNNRegressor(feature_columns=[...],
                     hidden_units=[128, 64, 32])
```


* Classification

```python
model = LinearClassifier(feature_columns=[...])
model = DNNClassifier(feature_columns=[...], hidden_units=[...])
```


## Lab 3: Machine Learning using tf.estimator

In this lab, you will

* Read from Pandas Dataframe into tf.constant
* Create feature columns for estimator
* Linear Regression with tf.Estimator framework
* Deep Neural Network regression
* Benchmark dataset

### Task 1. Launch Cloud Datalab

* Launch [Google Cloud Shell Code Editor](https://console.cloud.google.com/cloudshell/editor)

```shell
datalab create dataengvm --zone europe-north1-a
```

more details here [Task 1. Launch Cloud Datalab](#task-1-launch-cloud-datalab)


### Task 2. Clone repo into Cloud Datalab

```python
%bash
git clone https://github.com/GoogleCloudPlatform/training-data-analyst
cd training-data-analyst
```

### Task 3: Run the lab in the notebook


In Cloud Datalab, click on the Home icon, and then navigate to **datalab/training-data-analyst/courses/machine_learning/tensorflow**
or check [Github to see its content](https://github.com/GoogleCloudPlatform/training-data-analyst/tree/master/courses/machine_learning/tensorflow).

Open [**b_estimator.ipynb**](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/courses/machine_learning/tensorflow/b_estimator.ipynb).
Read the narrative and execute each cell in turn.


> Tried to run with Colab by replacing the Github link like with:
> 
> * "https://colab.research.google.com/" + replace "github.com" with "github" + "/GoogleCloudPlatform/training-data-analyst/blob/master/courses/machine_learning/tensorflow/b_estimator.ipynb"
> * Make a copy to your drive that you can run and modify. ([Here is mine!](https://colab.research.google.com/drive/1XqYk0QpTj6iHtyathmhZCzN3ADR1BAjK#scrollTo=ZEEQaTTIpBof))
> * You will need to enable billing and APIs in order to import Google packages and run them



#### Read data created in the previous lab

```python
# In CSV, label is the first column, after the features, followed by the key
CSV_COLUMNS = ['fare_amount', 'pickuplon','pickuplat','dropofflon','dropofflat','passengers', 'key']
FEATURES = CSV_COLUMNS[1:len(CSV_COLUMNS) - 1]
LABEL = CSV_COLUMNS[0]

df_train = pd.read_csv('./taxi-train.csv', header = None, names = CSV_COLUMNS)
df_valid = pd.read_csv('./taxi-valid.csv', header = None, names = CSV_COLUMNS)
```

#### Input function to read from Pandas Dataframe into tf.constant

```python
def make_input_fn(df, num_epochs):
  return tf.estimator.inputs.pandas_input_fn(
    x = df,
    y = df[LABEL],
    batch_size = 128,
    num_epochs = num_epochs,
    shuffle = True,
    queue_capacity = 1000,
    num_threads = 1
  )
```

#### Create feature columns for estimator


Here we create a numeric column for every feature.

```python
input_columns = [tf.feature_column.numeric_column(k) for k in FEATURES]
```


#### Linear Regression with tf.Estimator framework

```python
OUTDIR = 'taxi_trained'

model = tf.estimator.LinearRegressor(
      feature_columns = make_feature_cols(), model_dir = OUTDIR)

model.train(input_fn = make_input_fn(df_train, num_epochs = 10))
```

#### Evaluate on the validation data

(we should defer using the test data to after we have selected a final model).


```python
def print_rmse(model, name, df):
  metrics = model.evaluate(input_fn = make_input_fn(df, 1))
  print 'RMSE on {} dataset = {}'.format(name, np.sqrt(metrics['average_loss']))
print_rmse(model, 'validation', df_valid)
# RMSE on validation dataset = 10.6943283081
```

This is nowhere near our benchmark (RMSE of $6 or so on this data), but it serves to demonstrate what TensorFlow code looks like.

Let's use this model for prediction.

```python
import itertools
# Read saved model and use it for prediction
model = tf.estimator.LinearRegressor(
      feature_columns = make_feature_cols(), model_dir = OUTDIR)
preds_iter = model.predict(input_fn = make_input_fn(df_valid, 1))
print [pred['predictions'][0] for pred in list(itertools.islice(preds_iter, 5))]
# [9.272911, 9.276136, 9.370888, 9.46672, 9.370997]
```

This explains why the RMSE was so high -- the model essentially predicts the same amount for every trip.

Would a more complex model help? Let's try using a deep neural network. The code to do this is quite straightforward as well.

#### Deep Neural Network regression


```python
tf.logging.set_verbosity(tf.logging.INFO)
shutil.rmtree(OUTDIR, ignore_errors = True) # start fresh each time
model = tf.estimator.DNNRegressor(hidden_units = [32, 8, 2],
      feature_columns = make_feature_cols(), model_dir = OUTDIR)
model.train(input_fn = make_input_fn(df_train, num_epochs = 100));
print_rmse(model, 'validation', df_valid)
# RMSE on validation dataset = 11.6112670898
```

You can hope to get a better RMSE by training longer, i.e. increase the number of epoch: `num_epoch`.


We are not beating our benchmark with either model ... what's up? Well, we may be using TensorFlow for Machine Learning, but we are not yet using it well. That's what the rest of this course is about!

But, for the record, let's say we had to choose between the two models. We'd choose the one with the lower validation error. Finally, we'd measure the RMSE on the test data with this chosen model.

#### Benchmark dataset


```python
import datalab.bigquery as bq
import numpy as np
import pandas as pd


def create_query(phase, EVERY_N):
  """
  phase: 1 = train 2 = valid
  """
  base_query = """
SELECT
  (tolls_amount + fare_amount) AS fare_amount,
  CONCAT(STRING(pickup_datetime), STRING(pickup_longitude), STRING(pickup_latitude), STRING(dropoff_latitude), STRING(dropoff_longitude)) AS key,
  DAYOFWEEK(pickup_datetime)*1.0 AS dayofweek,
  HOUR(pickup_datetime)*1.0 AS hourofday,
  pickup_longitude AS pickuplon,
  pickup_latitude AS pickuplat,
  dropoff_longitude AS dropofflon,
  dropoff_latitude AS dropofflat,
  passenger_count*1.0 AS passengers,
FROM
  [nyc-tlc:yellow.trips]
WHERE
  trip_distance > 0
  AND fare_amount >= 2.5
  AND pickup_longitude > -78
  AND pickup_longitude < -70
  AND dropoff_longitude > -78
  AND dropoff_longitude < -70
  AND pickup_latitude > 37
  AND pickup_latitude < 45
  AND dropoff_latitude > 37
  AND dropoff_latitude < 45
  AND passenger_count > 0
  """

  if EVERY_N == None:
    if phase < 2:
      # Training
      query = "{0} AND ABS(HASH(pickup_datetime)) % 4 < 2".format(base_query)
    else:
      # Validation
      query = "{0} AND ABS(HASH(pickup_datetime)) % 4 == {1}".format(base_query, phase)
  else:
    query = "{0} AND ABS(HASH(pickup_datetime)) % {1} == {2}".format(base_query, EVERY_N, phase)
    
  return query

query = create_query(2, 100000)
df = bq.Query(query).to_dataframe()
print_rmse(model, 'benchmark', df)
# RMSE on benchmark dataset is 9.41
```






```python

```



```python

```







## Distributed TensorFlow models


### Write TensorFlow graphs and Training an evaluation loop

### Monitor ML training with TensorBoard

























## Other references


* [Angeliqa's notes](https://docs.google.com/document/d/1_G6F5qmjcklNOK8KBkUBD5gDHMLXrNjCX6yIBuR8iEA/edit?pli=1)
* [GCP Labs and demos for the course](https://github.com/GoogleCloudPlatform/training-data-analyst)
* [Cloud Datalab resources on Github](https://github.com/googledatalab/notebooks)


## Recommended extra courses

* [Machine learning with TensorFlow on GCP](https://www.coursera.org/specializations/machine-learning-tensorflow-gcp) (5 courses, 7+ weeks)
* Advanced machine learning with TensorFlow on GCP: ??
* https://ai.google/education/
* https://developers.google.com/machine-learning/crash-course/
