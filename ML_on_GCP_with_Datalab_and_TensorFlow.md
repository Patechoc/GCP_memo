# Memo for TensorFlow on Google Cloud Platform

## Content

      * [Content](#content)
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
            * [Working with Google Colaboratory instead of Google Cloud Datalab](#working-with-google-colaboratory-instead-of-google-cloud-datalab)
            * [Read data created in the previous lab](#read-data-created-in-the-previous-lab)
            * [Input function to read from Pandas Dataframe into tf.constant](#input-function-to-read-from-pandas-dataframe-into-tfconstant)
            * [Create feature columns for estimator](#create-feature-columns-for-estimator)
            * [Linear Regression with tf.Estimator framework](#linear-regression-with-tfestimator-framework)
            * [Evaluate on the validation data](#evaluate-on-the-validation-data)
            * [Deep Neural Network regression](#deep-neural-network-regression)
            * [Benchmark dataset](#benchmark-dataset)
      * [Lab 4: Refactoring to add batching and feature-creation](#lab-4-refactoring-to-add-batching-and-feature-creation)
         * [Task 1. Launch Cloud Datalab](#task-1-launch-cloud-datalab-3)
         * [Task 2. Clone repo into Cloud Datalab](#task-2-clone-repo-into-cloud-datalab-3)
         * [Task 3: Run the lab in the notebook](#task-3-run-the-lab-in-the-notebook-3)
            * [Refactor (#1) the input to read  data in batches (Big Data soon <g-emoji class="g-emoji" alias="100" fallback-src="https://assets-cdn.github.com/images/icons/emoji/unicode/1f4af.png">💯</g-emoji>)](#refactor-1-the-input-to-read--data-in-batches-big-data-soon-100)
            * [Refactor (#2) the way features are created](#refactor-2-the-way-features-are-created)
            * [Create and train the model](#create-and-train-the-model)
            * [Evaluate the model](#evaluate-the-model)
      * [Train and Evaluate Distributed TensorFlow models](#train-and-evaluate-distributed-tensorflow-models)
         * [Write TensorFlow graphs and Training an evaluation loop](#write-tensorflow-graphs-and-training-an-evaluation-loop)
         * [Monitor ML training with TensorBoard](#monitor-ml-training-with-tensorboard)
      * [Lab 5: Distributed training and monitoring (evaluation)](#lab-5-distributed-training-and-monitoring-evaluation)
         * [Task 1. Launch Cloud Datalab](#task-1-launch-cloud-datalab-4)
         * [Task 2. Clone repo into Cloud Datalab](#task-2-clone-repo-into-cloud-datalab-4)
         * [Task 3: Run the lab in the notebook](#task-3-run-the-lab-in-the-notebook-4)
            * [train_and_evaluate](#train_and_evaluate)
            * [Monitoring with TensorBoard](#monitoring-with-tensorboard)
      * [Deploying TF models to GCP &amp; Scaling with CloudML Engine](#deploying-tf-models-to-gcp--scaling-with-cloudml-engine)
         * [Why CloudML Engine](#why-cloudml-engine)
         * [Development workflow](#development-workflow)
      * [Lab 6: Deploying &amp; Scaling up your ML model using Cloud ML Engine](#lab-6-deploying--scaling-up-your-ml-model-using-cloud-ml-engine)
         * [Task 1. Launch Cloud Datalab](#task-1-launch-cloud-datalab-5)
         * [Task 2. Clone repo into Cloud Datalab](#task-2-clone-repo-into-cloud-datalab-5)
         * [Task 3: Verify that you have a Cloud Storage bucket](#task-3-verify-that-you-have-a-cloud-storage-bucket)
         * [Task 4: Run the lab in the notebook](#task-4-run-the-lab-in-the-notebook)
            * [Environment variables for project and bucket](#environment-variables-for-project-and-bucket)
            * [Packaging up the code](#packaging-up-the-code)
            * [Find absolute paths to your data](#find-absolute-paths-to-your-data)
            * [Running the Python module from the command-line](#running-the-python-module-from-the-command-line)
            * [Training locally using gcloud](#training-locally-using-gcloud)
            * [Training in the cloud by submit a training job using gcloud](#training-in-the-cloud-by-submit-a-training-job-using-gcloud)
            * [Deploy model](#deploy-model)
            * [Prediction](#prediction)
            * [Train on larger dataset](#train-on-larger-dataset)
            * [Run Cloud training on 1-million row dataset](#run-cloud-training-on-1-million-row-dataset)
      * [Improve ML through Feature Engineering, Hyperparameter tuning\x�sh](#improve-ml-through-feature-engineering-hyperparameter-tuningbash)
         * [What makes a good feature?](#what-makes-a-good-feature)
            * [Causality](#causality)
            * [Numeric](#numeric)
            * [Raw data to features](#raw-data-to-features)
            * [Categorical features (One-hot encoding)](#categorical-features-one-hot-encoding)
            * [Feature crosses](#feature-crosses)
            * [Bucketizing](#bucketizing)
            * [Model Architecture (Wide and Deep)](#model-architecture-wide-and-deep)
            * [Where to do feature engineering?](#where-to-do-feature-engineering)
      * [Lab 7: Feature Engineering](#lab-7-feature-engineering)
         * [Task 1. Launch Cloud Datalab](#task-1-launch-cloud-datalab-6)
         * [Task 2. Clone repo into Cloud Datalab](#task-2-clone-repo-into-cloud-datalab-6)
         * [Task 3: Verify that you have a Cloud Storage bucket](#task-3-verify-that-you-have-a-cloud-storage-bucket-1)
         * [Task 4: Run the lab in the notebook](#task-4-run-the-lab-in-the-notebook-1)
      * [Hyperparameter Tuning](#hyperparameter-tuning)
      * [Moral of the taxi fare use case](#moral-of-the-taxi-fare-use-case)
      * [ML abstraction](#ml-abstraction)
      * [Other references](#other-references)
      * [Recommended extra courses](#recommended-extra-courses)


---

## Definition

* **MSE** : the loss measure for regression models.
* **Cross Entropy**: the loss measure for a classification problem.
* **Accuracy**: a more intuitive measure of skill for classifiers with a balanced dataset.
* **Precision**: the accuracy when the classifier says "YES".
* **Recall**: the accuracy when the truth is "YES".

## Playing with ML

__Check paper notebook__

## Effective ML model

__Check paper notebook__

## What makes a good dataset

The dataset should cover all cases. Be purposeful: take to human expert to get right amount/type of data.

Make sure you have Negative examples and near-misses.: You have to know what kind of data will confuse a ML model. So ensure you have negative examples and ones that are so close that they will near misses.


Explore the data you have: fix problems. Find outliers and sometimes take them away while it is usually best to collect enough of those outliers to let your model learn from them as well.

### Error metrics

you need an error measure:

* square the error
* calculate the mean

You get the mean-square error!

* in regression problems: you sum over the difference squared between the true value and your estimate.
* in classification problems: the estimate is a real value between 0 and 1, while the true value is either 0 or 1. There the error measure is called the **cross-entropy** or [**log loss**](http://scikit-learn.org/stable/modules/generated/sklearn.metrics.log_loss.html).

### Accuracy

The cross-entropy is not very intuitive. A **confusion matrix** for a classification problem usually helps more to describe the performance of its model.

Gather the outcomes into a table:

| Truth \ Estimation | Cat               | No Cat            |
|--------------------|-------------------|-------------------|
| Cat                | **True Positive** | False Negative    |
| No Cat             | False Positive    | **True Negative** |

From the confusion matrix, you can calculate a measure of skill:

<a href="https://latex.codecogs.com/gif.latex?accuracy%20%3D%20%5Cfrac%7B%5Csum%20TP%20&plus;%20TN%7D%7B%5Csum%20all%20%5C%20images%7D" target="_blank"><img src="https://latex.codecogs.com/gif.latex?accuracy%20%3D%20%5Cfrac%7B%5Csum%20TP%20&plus;%20TN%7D%7B%5Csum%20all%20%5C%20images%7D" title="Accuracy" /></a>


If the dataset is not balanced, then accuracy starts to have a problem.

e.g. 1000 parking spaces, 990 are occupied and only 10 free. If your ML model identify only 1 free space, its accuracy is still really high: accuracy = (990+1)/1000 = 0.991 = 99.1%

If you have an unbalanced dataset, you want to look at **precision** and **recall**.

### Precision and Recall

precision = positive predictive value

<a href="https://latex.codecogs.com/gif.latex?precision%20%3D%20%5Cfrac%7B%5Csum%20TP%7D%7B%5Csum%20TP&plus;%20FP%7D" target="_blank"><img src="https://latex.codecogs.com/gif.latex?precision%20%3D%20%5Cfrac%7B%5Csum%20TP%7D%7B%5Csum%20TP&plus;%20FP%7D" title="Precision" /></a>

e.g. 1000 parking spaces, 990 are occupied and only 10 free. If your ML model identify only 1 free space, its precision = 1/(1+0) = 1 = 100%

Precision is one aspect for judging the performance of your model, but still not enough.

On an unbalanced dataset, you need to look at recall:

https://latex.codecogs.com/gif.latex?recall%20%3D%20%5Cfrac%7B%5Csum%20TP%7D%7B%5Csum%20TP%20&plus;%20FN%7D

e.g. 1000 parking spaces, 990 are occupied and only 10 free. If your ML model identify only 1 free space, recall = 1/ (10 + 0)  = 0.1 = 10%

> **Accuracy** is what you will use wit ha balanced dataset
> with an unbalanced dataset, you will need **Precision and Recall**.
>
> * You tend to use **Recall** when the things you are trying to find are _*very rare*_ (e.g. very few cats in your set of images).
>
> * You tend to use **Precision** when the things you are trying to find are _*very common*_ (e.g. very many cats in your set of images).



### Creating ML datasets

In the regression problem we solve below in the lab, the error measure is the mean-square error (MSE).

Ww try to predict the amount of taxi fare given the distance travelled.

The RMSE (root MSE) would provide intuitive value measured in dollars!

While a simple linear model might give a bad RMSE of 22. A fine tuned polynomial may give 0. But the latter won't generalize well (and **overfit** to the training data).

with new data, the linear model will provide similar "bad" RMSE, while the polynomial model a much worse RMSE.

The error on training data is too different from the error on the testing data.

We definitely need to split our dataset to be able to assess the complexity of our model. We improve the model as an **iterative process**:

* start with one set of algo/hyper-parameters
* check for overfitting?
    * NO: increase model complexity (and reduce your RMSE)
    * YES: you are done! (your previous RMSE was optinal, use that previous model).


To evaluate the performance of your model, we will split the original dataset in 3:

* **training data**: the training set is used to train a given model.
* **validation data**: the validation set is used to choose between models (for instance, does a random forest or a neural net work better for your problem? do you want a random forest with 40 trees or 50 trees?).
* **test data**: the test set tells you how you’ve done. If you’ve tried out a lot of different models, you may get one that does well on your validation set just by chance, and having a test set helps make sure that is not the case.

You could eventually use cross-validation if your data is scarce (not enough data), which will split 10 times your full dataset into training/test sets.

A [good article on **How (and Why) to Create a Good Validation Set**](https://www.kdnuggets.com/2017/11/create-good-validation-set.html).

----------------------------------------------------------------------------------

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


----------------------------------------------------------------------------------

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


----------------------------------------------------------------------------------

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

----------------------------------------------------------------------------------

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

----------------------------------------------------------------------------------

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


#### Working with Google Colaboratory instead of Google Cloud Datalab

> Tried to run with Colab by replacing the Github link like with:
> 
> * "https://colab.research.google.com/" + replace "github.com" with "github" + "/GoogleCloudPlatform/training-data-analyst/blob/master/courses/machine_learning/tensorflow/b_estimator.ipynb"
> * Make a copy to your drive that you can run and modify. ([Here is mine!](https://colab.research.google.com/drive/1XqYk0QpTj6iHtyathmhZCzN3ADR1BAjK#scrollTo=ZEEQaTTIpBof))
> * You will need to enable billing and APIs in order to import Google packages and run them
>
> **I haven't tried it yet. Need to investigate how the billing part works here!** :unamused:



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

You can hope to get a better RMSE by training longer, i.e. increase the number of epoch: `num_epoch`.


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

----------------------------------------------------------------------------------

## Lab 4: Refactoring to add batching and feature-creation

In this lab, you will perform the following tasks:

* Refactor the input
* Refactor the way the features are created
* Create and train the model
* Evaluate model

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

Open [**c_batched.ipynb**](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/courses/machine_learning/tensorflow/c_batched.ipynb).
Read the narrative and execute each cell in turn.

#### Refactor (#1) the input to read  data in batches (Big Data soon :100:)

We continue reading the same small dataset, but refactor our ML pipeline in two small, but significant, ways

Read data created in Lab-1a, but this time make it more general and performant. Instead of using Pandas, we will use TensorFlow's Dataset API (`tf.data.TextLineDataset(file_list).map(decode_csv)`).

`read_dataset()` now reads the data in chuncks, it reads CSV files directly rather than using Pandas.

* we take the path to the file `filename`
* we glob it (`tf.gfile.Glob`), like a wildcard match. So `./taxi-train.*` works as well as `./taxi-train.csv`.
* out of this file list, create a `tf.data.TextLineDataset(file_list).map(decode_csv)` out of this list, and for every line, call the `decode_csv()` function.
* This `decode_csv` function calls the `tf.decode_csv()` function that takes care of applying the right type (integer, float, ...) to the column values and apply the respective default values in case of missing ones in the CSV file. It returns (features, labels)
* We repeat it and shuffle it as necessary (as before) and that becomes the way we read our data.
 
```python
CSV_COLUMNS = ['fare_amount', 'pickuplon','pickuplat','dropofflon','dropofflat','passengers', 'key']
LABEL_COLUMN = 'fare_amount'
DEFAULTS = [[0.0], [-74.0], [40.0], [-74.0], [40.7], [1.0], ['nokey']]

def read_dataset(filename, mode, batch_size = 512):
  def _input_fn():
    def decode_csv(value_column):
      columns = tf.decode_csv(value_column, record_defaults = DEFAULTS)
      features = dict(zip(CSV_COLUMNS, columns))
      label = features.pop(LABEL_COLUMN)
      return features, label

    # Create list of files that match pattern
    file_list = tf.gfile.Glob(filename)

    # Create dataset from file list
    dataset = tf.data.TextLineDataset(file_list).map(decode_csv)
    if mode == tf.estimator.ModeKeys.TRAIN:
        num_epochs = None # indefinitely
        dataset = dataset.shuffle(buffer_size = 10 * batch_size)
    else:
        num_epochs = 1 # end-of-input after this

    dataset = dataset.repeat(num_epochs).batch(batch_size)
    return dataset.make_one_shot_iterator().get_next()
  return _input_fn
    

def get_train():
  return read_dataset('./taxi-train.csv', mode = tf.estimator.ModeKeys.TRAIN)

def get_valid():
  return read_dataset('./taxi-valid.csv', mode = tf.estimator.ModeKeys.EVAL)

def get_test():
  return read_dataset('./taxi-test.csv', mode = tf.estimator.ModeKeys.EVAL)
```


#### Refactor (#2) the way features are created

For now, pass these through (same as previous lab). However, refactoring this way will enable us to break the one-to-one relationship between inputs and features.

We haven't added any new features yet, but we now have the mechanism in palce to add more features in the future.

```python
INPUT_COLUMNS = [
    tf.feature_column.numeric_column('pickuplon'),
    tf.feature_column.numeric_column('pickuplat'),
    tf.feature_column.numeric_column('dropofflat'),
    tf.feature_column.numeric_column('dropofflon'),
    tf.feature_column.numeric_column('passengers'),
]

def add_more_features(feats):
  # Nothing to add (yet!)
  return feats

feature_cols = add_more_features(INPUT_COLUMNS)
```


#### Create and train the model

Note that we train for num_steps * batch_size examples.

```python
tf.logging.set_verbosity(tf.logging.INFO)
OUTDIR = 'taxi_trained'
shutil.rmtree(OUTDIR, ignore_errors = True) # start fresh each time
model = tf.estimator.LinearRegressor(
      feature_columns = feature_cols, model_dir = OUTDIR)
model.train(input_fn = get_train(), steps = 1000);  # TODO: change the name of input_fn as needed
```



#### Evaluate the model

As before, evaluate on the validation data. We'll do the third refactoring (to move the evaluation into the training loop) in the next lab.


```python
def print_rmse(model, name, input_fn):
  metrics = model.evaluate(input_fn = input_fn, steps = 1)
  print 'RMSE on {} dataset = {}'.format(name, np.sqrt(metrics['average_loss']))
print_rmse(model, 'validation', get_valid())
...
INFO:tensorflow:Saving dict for global step 1000: average_loss = 129.05605, global_step = 1000, loss = 66076.695
RMSE on validation dataset = 11.3602838516
```

RMSE as bad as before. refactoring worked :)

----------------------------------------------------------------------------------

## Train and Evaluate Distributed TensorFlow models

### Write TensorFlow graphs and Training an evaluation loop

We have seen how to distribute reading of data with TensorFlow.

For distributing training and evaluation, we need to **refactor distribution and monitoring**:

- what happends if a machine crashes during training? you would like to resume training instead of starting from scratch.
- you want to monitor the training job to stop it when it has converged
- you want the possiblity to retrain a model with fresh data, but not starting from scratch

What TensorFlow does for you:

* **shuffle the data between workers during training**: so that different workers converge differently with gradient descent seeing the same data in a different order.
* Estimators come with a cool **`train_and_evaluate()` method** which handles all the hardwork of writing fault-tolerant code.
    * distribute the graphs
    * share variables
    * evaluate once in a while
    * ...
    and to do this, it creates **checkpoint files** whic allow to recover from failures and to save output summary to **TensorBoard**.

```python
tf.estimator.train_and_evaluate(estimator,
				train_spec,
				eval_spec)
```

**Train Spec** takes:

* the same input function (`input_fn`) as before spitting a tuple `(features, labels)`,
* but also a parameter that is **measured in steps** (`max_steps`).

> With distributed models in production, think `steps` not `epoch`
>
> when training with fresh data, we want to resume from a checkpoint corresponding to an earlier number of steps, a not epochs for efficient recovery!

**Eval Spec** controls the **evaluation** and the **checkpointing** of the model since they haååen at the same time.

* the training loop saves the model into a checkpoint
* the eval loop restores a model from a checkpoint and uses it to evaluate the model.
* `throttle_secs = 600`: evaluation made every 600 seconds.
* `start_delay_secs = 60`: starting evaluation 60 sec after begining of the training job.

### Monitor ML training with TensorBoard

**Monitor your training** by changing the logging level from `WARN` to `INFO`/`DEBUG`.

```python
tf.logging.set_verbosity(tf.logging.INFO)
```


Use **TensorBoard** to monitor training from a GUI: it helps visualize TF models evaluation.


## Lab 5: Distributed training and monitoring (evaluation)

In this lab, you will perform the following tasks:

* Create features out of input data
* Train and evaluate
* Monitor with Tensorboard

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

Open [**d_traineval.ipynb**](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/courses/machine_learning/tensorflow/d_traineval.ipynb).
Read the narrative and execute each cell in turn.

In this notebook, we refactor to call `train_and_evaluate()` instead of hand-coding our ML pipeline. This allows us to carry out evaluation as part of our training loop instead of as a separate step. It also adds in failure-handling that is necessary for distributed training capabilities.

We also use **TensorBoard** to monitor the training.

####  `train_and_evaluate`


```python
def serving_input_fn():
  feature_placeholders = {
    'pickuplon' : tf.placeholder(tf.float32, [None]),
    'pickuplat' : tf.placeholder(tf.float32, [None]),
    'dropofflat' : tf.placeholder(tf.float32, [None]),
    'dropofflon' : tf.placeholder(tf.float32, [None]),
    'passengers' : tf.placeholder(tf.float32, [None]),
  }
  features = {
      key: tf.expand_dims(tensor, -1)
      for key, tensor in feature_placeholders.items()
  }
  return tf.estimator.export.ServingInputReceiver(features, feature_placeholders)
```


Below we define:

* an estimator: `tf.estimator.LinearRegressor()
* a `train_spec`: reading `taxi-train.csv` and saying wit hhow many steps we want to train
* an `eval_spec`: to evaluate on `taxi-valid.csv` and how often should you evaluate: here once every 10 seconds and start evaluating about a second in.
* the `eval_spec` also include an `exporter`, crucial for deploying our model (because we're going to basically export this model with a serving input function that looks for JSON data.)

```python
def train_and_evaluate(output_dir, num_train_steps):
  estimator = tf.estimator.LinearRegressor(
                       model_dir = output_dir,
                       feature_columns = feature_cols)
  train_spec=tf.estimator.TrainSpec(
                       input_fn = read_dataset('./taxi-train.csv', mode = tf.estimator.ModeKeys.TRAIN),
                       max_steps = num_train_steps)
  exporter = tf.estimator.LatestExporter('exporter', serving_input_fn)
  eval_spec=tf.estimator.EvalSpec(
                       input_fn = read_dataset('./taxi-valid.csv', mode = tf.estimator.ModeKeys.EVAL),
                       steps = None,
                       start_delay_secs = 1, # start evaluating after N seconds
                       throttle_secs = 10,  # evaluate every N seconds
                       exporters = exporter)
  tf.estimator.train_and_evaluate(estimator, train_spec, eval_spec)
```


```python 
# Run training    
OUTDIR = 'taxi_trained'
shutil.rmtree(OUTDIR, ignore_errors = True) # start fresh each time
train_and_evaluate(OUTDIR, num_train_steps = 5000)
```

We haven't changed the RMSE yet as we haven't change the amount  of input data yet.


#### Monitoring with TensorBoard

```python
from google.datalab.ml import TensorBoard
TensorBoard().start('./taxi_trained')
TensorBoard().list()
```


```python
# to stop TensorBoard
for pid in TensorBoard.list()['pid']:
    TensorBoard().stop(pid)
    print 'Stopped TensorBoard with pid {}'.format(pid)
```

----------------------------------------------------------------------------------

## Deploying TF models to GCP & Scaling with CloudML Engine


**CloudML Engine** is a serverless engine for training and deploying TensorFlow models.

* Fetch inputs
* Data Pre-processing
* Feature creation
* Train model (hyper-parameter tuning)
* Move to TensorFlow models
* Deploy model to a Web Application (as REST API) to make predictions


**"Training Service Queue"**: your prediction inputs will often differ from the data you use for training. (e.g. maybe the average of some column has shifted, or the variance has grown over time). Detecting it requires continuous data collection and re-examination.

### Why CloudML Engine

CloudML is:

* **repeatable**: it takes care of bookkeeping (scaling of parameters, order of pipeline, ...). It helps handle Training Service Queue.
* **scalable**: it helps distribute your training, hyper-parameter tuning, ...

CloudML will scale automatically to reach the required number of queries-per-seconds.

Notebooks from Datalab or Kaggle's kernels helps get started quickly and iterate smoothly when developing your model: explore, training and evaluate your jobs.


### Development workflow

1. Clean, split and prepare your engineered features on your training data
1. Put your data on an online source that CloudML can access (e.g. Cloud Storage)
1. Use TensorFlow to create computation graph and training application
1. Package your trainer application
1. Configure and start a Cloud ML Engine job

When sending jobs to CloudML, split most of the logic in `task.py` and `model.py`:

* `task.py`: entry point to your code that CloudML will start and knows job-level's argument
* `model.py`: during the core machine learning, `task.py` will call `moddel.py`. It contains the ML model in TensorFlow
    * training and evaluation input functions (`read_dataset()`)
    * Feature columns (`INPUT_COLUMNS`)
    * feature engineering (`def add_more_features(feats)`)
    * Serving input function (`def serving _input_fn`)
    * train and evaluation loop (`def train_and_evaluate()`)


Package up TensorFlow model **as Python package**. So verify that the model works as a Python package

```shell
export PYTHONPATH=${PYTHONPATH}:/somedir/taxifare
python -m trainer.task \
  --train_data_path="/somedir/datasets/*train*" \
  --eval_data_paths="/somedir/datasets/*valid*" \
  --output_dir="/somedir/output" \
  --train_steps=100 --job-dir=/tmp
```

Then use `gcloud` command to locally test your code for quick sanity checks: 

```shell
gcloud ml-engine local train \
  --module-name=trainer.task \
  --package-path=/somedir/taxifare/trainer \
  ...
  --train_data_path ...
```

then use `gcloud` to submit your training job on the cloud. 

```shell
gcloud ml-engine jobs submit training $JOBNAME \
  --region=$REGION \
  --module-name=trainer.task \
  --job-dir=$OUTPUT --staging-bucket=gs://$BUCKET \
  -- scale-tier=BASIC \
  ...
```

Cloud ML Engine scale tier can be: `BASIC`, `STANDARD_1`, `BASIC_GPU`, `BASIC_TPU` (beta),  or [others](https://cloud.google.com/ml-engine/docs/tensorflow/machine-typeshttps://cloud.google.com/ml-engine/docs/tensorflow/machine-types).

With CloudML, our prediction model is available as a microservice through a REST API.

When you deploy the model, you export it with a **"serving input function"**. Can't you use the same input function as for training.

While your training input function was taking in CSV files, your Serving inpout function, as a REST API, will have to ingest JSON traffic. So we need a specific Serving input function to be able to read in JSON data (and of course, since it's prediction, this function doesn't need labels: y).

Once you have the exported model with this Serving input function, you can construct a microservice, either from the GCP web console or scripted out with `gcloud`:

```shell
MODEL_NAME="taxifare"
MODEL_VERSION="v1"
MODEL_LOCATION="gs://${BUCKET}/taxifare/export/exporter/983748293"

gcloud ml-engine models create ${MODEL_NAME} --regions $REGION
gcloud ml-engine versions create ${MODEL_VERSION} --model ${MODEL_NAME} --origin ${MODEL_LOCATION}
```

The Client code can make REST calls to your prediction model:

```python
token = GoogleCredentials.get_application_default().get_access_token().access_token
api = 'https://ml.googleapis.com/v1/projects/{}/models/{}/versions/{}:predict'.format(PROJECT, MODEL_NAME, MODEL_VERSION)

headers = {'Authorization': 'Bearer ' + token }
data = {
   'instances' : [
     {
     'pickuplon': -73.321432,
     'pickuplat': 40.21932,
     }

    ]
}
response = requests.post(api, json=data, headers=headers)
print(response.content)
```


----------------------------------------------------------------------------------


## Lab 6: Deploying & Scaling up your ML model using Cloud ML Engine

In this lab, you will perform the following tasks:

* Package up the code
* Find absolute paths to data
* Run the Python module from the command line
* Run locally using gcloud
* Submit training job using gcloud
* Deploy model
* Prediction
* Train on a 1-million row dataset

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

### Task 3: Verify that you have a Cloud Storage bucket

This lab notebook requires you to provide a Project ID, Bucket Name, and Bucket Region. If not create them.

```shell
$ echo $DEVSHELL_PROJECT_ID
qwiklabs-gcp-7693c01b6cf22dd2
$
$ echo $BUCKET
$ BUCKET="qwiklabs-gcp-7693c01b6cf22dd2"
$ echo $BUCKET
qwiklabs-gcp-7693c01b6cf22dd2
$
```


### Task 4: Run the lab in the notebook

In Cloud Datalab, click on the Home icon, and then navigate to **datalab/training-data-analyst/courses/machine_learning/cloudmle**
or check [Github to see its content](https://github.com/GoogleCloudPlatform/training-data-analyst/tree/master/courses/machine_learning/cloudmle).

Open [**cloudmle.ipynb**](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/courses/machine_learning/cloudmle/cloudmle.ipynb).

Read the narrative and execute each cell in turn.

In this notebook, we take a previously developed TensorFlow model to predict taxifare rides and package it up so that it can be run in Cloud ML Engine (Cloud MLE).

For now, we'll run this on a small dataset. The model that was developed is rather simplistic, and therefore, the accuracy of the model is not great either. However, this notebook illustrates how to package up a TensorFlow model to run it within Cloud ML.

Later in the course, we will look at ways to make a more effective machine learning model.

#### Environment variables for project and bucket

Note that:

1. Your project id is the __unique__ string that identifies your project (not the project name). You can find this from the GCP Console dashboard's Home page. 
2. Cloud training often involves saving and restoring model files. If you don't have a bucket already, I suggest that you create one from the GCP console (because it will dynamically check whether the bucket name you want is available). A common pattern is to prefix the bucket name by the project id, so that it is unique. Also, for cost reasons, you might want to use a single region bucket.

```python
import os
PROJECT = 'qwiklabs-gcp-7693c01b6cf22dd2' # REPLACE WITH YOUR PROJECT ID
REGION = 'us-central1' # Choose an available region for Cloud MLE from https://cloud.google.com/ml-engine/docs/regions.
BUCKET = PROJECT # REPLACE WITH YOUR BUCKET NAME. Use a regional bucket in the region you selected.
```

```python
# for bash
os.environ['PROJECT'] = PROJECT
os.environ['BUCKET'] = BUCKET
os.environ['REGION'] = REGION
os.environ['TFVERSION'] = '1.4'  # Tensorflow version
```

```python
%bash
gcloud config set project $PROJECT
gcloud config set compute/region $REGION
```

Allow the Cloud ML Engine service account to read/write to the bucket containing training data.

```python
%bash
PROJECT_ID=$PROJECT
AUTH_TOKEN=$(gcloud auth print-access-token)
SVC_ACCOUNT=$(curl -X GET -H "Content-Type: application/json" \
    -H "Authorization: Bearer $AUTH_TOKEN" \
    https://ml.googleapis.com/v1/projects/${PROJECT_ID}:getConfig \
    | python -c "import json; import sys; response = json.load(sys.stdin); \
    print response['serviceAccount']")

echo "Authorizing the Cloud ML Service account $SVC_ACCOUNT to access files in $BUCKET"
gsutil -m defacl ch -u $SVC_ACCOUNT:R gs://$BUCKET
gsutil -m acl ch -u $SVC_ACCOUNT:R -r gs://$BUCKET  # error message (if bucket is empty) can be ignored
gsutil -m acl ch -u $SVC_ACCOUNT:W gs://$BUCKET
## Authorizing the Cloud ML Service account service-55609621383@cloud-ml.google.com.iam.gserviceaccount.com to access files in qwiklabs-gcp-7693c01b6cf22dd2
```


#### Packaging up the code

Take your code and put into a standard Python package structure.

`model.py` and `task.py` contain the Tensorflow code from earlier (explore the directory structure).

```python
!find taxifare
taxifare
taxifare/PKG-INFO
taxifare/setup.cfg
taxifare/setup.py
taxifare/trainer
taxifare/trainer/.gitignore
taxifare/trainer/.model.py.swp
taxifare/trainer/__init__.py
taxifare/trainer/__init__.pyc
taxifare/trainer/model.py
taxifare/trainer/model.pyc
taxifare/trainer/task.py
taxifare/trainer.egg-info
taxifare/trainer.egg-info/dependency_links.txt
taxifare/trainer.egg-info/PKG-INFO
taxifare/trainer.egg-info/SOURCES.txt
taxifare/trainer.egg-info/top_level.txt
```

```python
!cat taxifare/trainer/model.py
#!/usr/bin/env python

# Copyright 2017 Google Inc. All Rights Reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

from __future__ import absolute_import
from __future__ import division
from __future__ import print_function

import tensorflow as tf
import numpy as np
import shutil

tf.logging.set_verbosity(tf.logging.INFO)

# List the CSV columns
CSV_COLUMNS = ['fare_amount', 'pickuplon','pickuplat','dropofflon','dropofflat','passengers', 'key']

#Choose which column is your label
LABEL_COLUMN = 'fare_amount'

# Set the default values for each CSV column in case there is a missing value
DEFAULTS = [[0.0], [-74.0], [40.0], [-74.0], [40.7], [1.0], ['nokey']]

# Create an input function that stores your data into a dataset
def read_dataset(filename, mode, batch_size = 512):
    def _input_fn():
        def decode_csv(value_column):
            columns = tf.decode_csv(value_column, record_defaults = DEFAULTS)
            features = dict(zip(CSV_COLUMNS, columns))
            label = features.pop(LABEL_COLUMN)
            return features, label
    
        # Create list of files that match pattern
        file_list = tf.gfile.Glob(filename)

        # Create dataset from file list
        dataset = tf.data.TextLineDataset(file_list).map(decode_csv)
        
        if mode == tf.estimator.ModeKeys.TRAIN:
            num_epochs = None # indefinitely
            dataset = dataset.shuffle(buffer_size = 10 * batch_size)
        else:
            num_epochs = 1 # end-of-input after this

        dataset = dataset.repeat(num_epochs).batch(batch_size)
        return dataset.make_one_shot_iterator().get_next()
    return _input_fn

# Define your feature columns
INPUT_COLUMNS = [
    tf.feature_column.numeric_column('pickuplon'),
    tf.feature_column.numeric_column('pickuplat'),
    tf.feature_column.numeric_column('dropofflat'),
    tf.feature_column.numeric_column('dropofflon'),
    tf.feature_column.numeric_column('passengers'),
]

# Create a function that will augment your feature set
def add_more_features(feats):
    # Nothing to add (yet!)
    return feats

feature_cols = add_more_features(INPUT_COLUMNS)

# Create your serving input function so that your trained model will be able to serve predictions
def serving_input_fn():
    feature_placeholders = {
        column.name: tf.placeholder(tf.float32, [None]) for column in INPUT_COLUMNS
    }
    features = {
        key: tf.expand_dims(tensor, -1)
        for key, tensor in feature_placeholders.items()
    }
    return tf.estimator.export.ServingInputReceiver(features, feature_placeholders)

# Create an estimator that we are going to train and evaluate
def train_and_evaluate(args):
    estimator = tf.estimator.DNNRegressor(
        model_dir = args['output_dir'],
        feature_columns = feature_cols,
        hidden_units = args['hidden_units'])
    train_spec = tf.estimator.TrainSpec(
        input_fn = read_dataset(args['train_data_paths'],
                                batch_size = args['train_batch_size'],
                                mode = tf.estimator.ModeKeys.TRAIN),
        max_steps = args['train_steps'])
    exporter = tf.estimator.LatestExporter('exporter', serving_input_fn)
    eval_spec = tf.estimator.EvalSpec(
        input_fn = read_dataset(args['eval_data_paths'],
                                batch_size = 10000,
                                mode = tf.estimator.ModeKeys.EVAL),
        steps = None,
        start_delay_secs = args['eval_delay_secs'],
        throttle_secs = args['min_eval_frequency'],
        exporters = exporter)
    tf.estimator.train_and_evaluate(estimator, train_spec, eval_spec)
```
#### Find absolute paths to your data

Note the absolute paths below. /content is mapped in Datalab to where the home icon takes you

```python
%bash
echo $PWD
rm -rf $PWD/taxi_trained
cp $PWD/../tensorflow/taxi-train.csv .
cp $PWD/../tensorflow/taxi-valid.csv .
head -1 $PWD/taxi-train.csv
head -1 $PWD/taxi-valid.csv
/content/datalab/notebooks/ASL/training-data-analyst/courses/machine_learning/deepdive/03_tensorflow
12.0,-73.987625,40.750617,-73.971163,40.78518,1,0
6.0,-74.013667,40.713935,-74.007627,40.702992,2,0
```


#### Running the Python module from the command-line

```python
%bash
rm -rf taxifare.tar.gz taxi_trained
export PYTHONPATH=${PYTHONPATH}:${PWD}/taxifare
python -m trainer.task \
   --train_data_paths="${PWD}/taxi-train*" \
   --eval_data_paths=${PWD}/taxi-valid.csv  \
   --output_dir=${PWD}/taxi_trained \
   --train_steps=1000 --job-dir=./tmp
```

```python
%bash
ls $PWD/taxi_trained/export/exporter/
```

There is a timestamp, so something has been exported.

In fact we can write out an example input:

```python
%writefile ./test.json
{"pickuplon": -73.885262,"pickuplat": 40.773008,"dropofflon": -73.987232,"dropofflat": 40.732403,"passengers": 2}
```

Even though we have quite deployed it, we can use ml-engine local predict method to make sure that things will actually work, passing the `test.json` file we just manually made: 

```python
%bash
model_dir=$(ls ${PWD}/taxi_trained/export/exporter)
gcloud ml-engine local predict \
    --model-dir=${PWD}/taxi_trained/export/exporter/${model_dir} \
    --json-instances=./test.json
```

we get back that it is about 13 dollars: `PREDICTIONS [13.385661125183105]`


#### Training locally using gcloud

```python
%bash
rm -rf taxifare.tar.gz taxi_trained
gcloud ml-engine local train \
   --module-name=trainer.task \
   --package-path=${PWD}/taxifare/trainer \
   -- \
   --train_data_paths=${PWD}/taxi-train.csv \
   --eval_data_paths=${PWD}/taxi-valid.csv  \
   --train_steps=1000 \
   --output_dir=${PWD}/taxi_trained 
```

When I ran it (due to random seeds, your results will be different), the average_loss (Mean Squared Error) on the evaluation dataset was 187, meaning that the RMSE was around 13.

```python
from google.datalab.ml import TensorBoard
TensorBoard().start('{}/taxi_trained'.format(os.environ['PWD']))
```

```python
for pid in TensorBoard.list()['pid']:
  TensorBoard().stop(pid)
  print 'Stopped TensorBoard with pid {}'.format(pid)
```

If the above step (to stop TensorBoard) appears stalled, just move on to the next step. You don't need to wait for it to return.

```python
!ls $PWD/taxi_trained
```

#### Training in the cloud by submit a training job using gcloud


First copy the training data to the cloud. Then, launch a training job.

After you submit the job, go to the cloud console (http://console.cloud.google.com) and select **Machine Learning | Jobs** to monitor progress.

**Note:** Don't be concerned if the notebook stalls (with a blue progress bar) or returns with an error about being unable to refresh auth tokens. This is a long-lived Cloud job and work is going on in the cloud. Use the Cloud Console link (above) to monitor the job.


```python
%bash
echo $BUCKET
gsutil -m rm -rf gs://${BUCKET}/taxifare/smallinput/
gsutil -m cp ${PWD}/*.csv gs://${BUCKET}/taxifare/smallinput/
```

```python
%%bash
OUTDIR=gs://${BUCKET}/taxifare/smallinput/taxi_trained
JOBNAME=lab3a_$(date -u +%y%m%d_%H%M%S)
echo $OUTDIR $REGION $JOBNAME
gsutil -m rm -rf $OUTDIR
gcloud ml-engine jobs submit training $JOBNAME \
   --region=$REGION \
   --module-name=trainer.task \
   --package-path=${PWD}/taxifare/trainer \
   --job-dir=$OUTDIR \
   --staging-bucket=gs://$BUCKET \
   --scale-tier=BASIC \
   --runtime-version=$TFVERSION \
   -- \
   --train_data_paths="gs://${BUCKET}/taxifare/smallinput/taxi-train*" \
   --eval_data_paths="gs://${BUCKET}/taxifare/smallinput/taxi-valid*"  \
   --output_dir=$OUTDIR \
   --train_steps=10000
```


Don't be concerned if the notebook appears stalled (with a blue progress bar) or returns with an error about being unable to refresh auth tokens. This is a long-lived Cloud job and work is going on in the cloud.

**Use the Cloud Console link to monitor the job and do NOT proceed until the job is done.**


#### Deploy model

Find out the actual name of the subdirectory where the model is stored and use it to deploy the model. Deploying model will take up to 5 minutes.

```python
%bash
gsutil ls gs://${BUCKET}/taxifare/smallinput/taxi_trained/export/exporter
```

```python
%bash
MODEL_NAME="taxifare"
MODEL_VERSION="v1"
MODEL_LOCATION=$(gsutil ls gs://${BUCKET}/taxifare/smallinput/taxi_trained/export/exporter | tail -1)
echo "Run these commands one-by-one (the very first time, you'll create a model and then create a version)"
#gcloud ml-engine versions delete ${MODEL_VERSION} --model ${MODEL_NAME}
#gcloud ml-engine models delete ${MODEL_NAME}
gcloud ml-engine models create ${MODEL_NAME} --regions $REGION
gcloud ml-engine versions create ${MODEL_VERSION} --model ${MODEL_NAME} --origin ${MODEL_LOCATION} --runtime-version $TFVERSION
```

Run these commands one-by-one (the very first time, you'll create a model and then create a version)

#### Prediction


```python
%bash
gcloud ml-engine predict --model=taxifare --version=v1 --json-instances=./test.json
```

```python
from googleapiclient import discovery
from oauth2client.client import GoogleCredentials
import json

credentials = GoogleCredentials.get_application_default()
api = discovery.build('ml', 'v1', credentials=credentials,
            discoveryServiceUrl='https://storage.googleapis.com/cloud-ml/discovery/ml_v1_discovery.json')

request_data = {'instances':
  [
      {
        'pickuplon': -73.885262,
        'pickuplat': 40.773008,
        'dropofflon': -73.987232,
        'dropofflat': 40.732403,
        'passengers': 2,
      }
  ]
}

parent = 'projects/%s/models/%s/versions/%s' % (PROJECT, 'taxifare', 'v1')
response = api.projects().predict(body=request_data, name=parent).execute()
print "response={0}".format(response)
```


#### Train on larger dataset

In the next chapter (on feature engineering), we will avoid all this manual processing by using Cloud Dataflow.

Go to http://bigquery.cloud.google.com/ and type the query in `LEGACY` mode (This query will process 74.35 GB when run.):

```sql
SELECT
  (tolls_amount + fare_amount) AS fare_amount,
  pickup_longitude AS pickuplon,
  pickup_latitude AS pickuplat,
  dropoff_longitude AS dropofflon,
  dropoff_latitude AS dropofflat,
  passenger_count*1.0 AS passengers,
  'nokeyindata' AS key
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
  AND ABS(HASH(pickup_datetime)) % 1000 == 1
```

Query complete (12.024 sec elapsed, 74.35 GB processed).

Note that this is now **1,000,000 rows** (i.e. **100x the original dataset**).

Export this to CSV using the following steps (Note that I have already done this and made the resulting GCS data publicly available, so you don't need to do it.):

1. Click on the **"Save As Table"** button and note down the name of the dataset and table (`patrick_taxiFare` dataset, `taxifare_1millions_rows`).
1. On the BigQuery console, find the newly exported table in the left-hand-side menu, and click on the name.
1. Click on **"Export Table"**
1. Supply your bucket name and give it the name **train.csv** (for example: gs://cloud-training-demos-ml/taxifare/ch3/train.csv). Note down what this is. Wait for the job to finish (look at the "Job History" on the left-hand-side menu)
1. In the query above, change the final "== 1" to "== 2" and export this to Cloud Storage as valid.csv (e.g. gs://cloud-training-demos-ml/taxifare/ch3/valid.csv)
1. Download the two files, remove the header line and upload it back to GCS.


#### Run Cloud training on 1-million row dataset

This took 60 minutes and uses as input 1-million rows.

The model is exactly the same as above. The only changes are to the input (to use the larger dataset) and to the Cloud MLE tier (to use STANDARD_1 instead of BASIC -- STANDARD_1 is approximately 10x more powerful than BASIC).

At the end of the training the loss was 32, but the RMSE (calculated on the validation dataset) was stubbornly at 9.03. So, simply adding more data doesn't help. We are not beating the benchmark!!

Check the next part to see how to beat it using feature engineering.

```python
%%bash

XXXXX  this takes 60 minutes. if you are sure you want to run it, then remove this line.

OUTDIR=gs://${BUCKET}/taxifare/ch3/taxi_trained
JOBNAME=lab3a_$(date -u +%y%m%d_%H%M%S)
CRS_BUCKET=cloud-training-demos # use the already exported data
echo $OUTDIR $REGION $JOBNAME
gsutil -m rm -rf $OUTDIR
gcloud ml-engine jobs submit training $JOBNAME \
   --region=$REGION \
   --module-name=trainer.task \
   --package-path=${PWD}/taxifare/trainer \
   --job-dir=$OUTDIR \
   --staging-bucket=gs://$BUCKET \
   --scale-tier=STANDARD_1 \
   --runtime-version=$TFVERSION \
   -- \
   --train_data_paths="gs://${CRS_BUCKET}/taxifare/ch3/train.csv" \
   --eval_data_paths="gs://${CRS_BUCKET}/taxifare/ch3/valid.csv"  \
   --output_dir=$OUTDIR \
   --train_steps=100000
```


----------------------------------------------------------------------------------

## Improve ML through Feature Engineering, Hyperparameter tuning%%bash

How to create good features?

### What makes a good feature?

- relate you feature to the objective (remove any spurious correlations)
- should be known at prediction time, since you won't predict from data you can not have later.
- has to be numeric, with meaningful magnitude
- has enough samples (Lak's rule of thumb: at least 5 samples of each value)
- brings human insight to the problem

#### Causality

feature value should be known at the time of prediction.

#### Numeric

All features must be numeric and have meaningful magnitude

#### Raw data to features

Don't forget to remove columns like "Transaction ID" if not useful. They will mislead the model.

#### Categorical features (One-hot encoding)

`tf.feature_column.categorical_column_with_vocabulary_list(...)`

Should be part of preprocessing.

e.g. for taxi fare, "HOURS-OF-DAY" is not numeric, it is categorical!

> Remove one column to keep (N-1) of the features in total to avoid correlation.

#### Feature crosses

How to bring human insight into a problem.

Adding more (sometimes artificial) columns.

e.g. a third column which is the concatenation of 2 others (without necessarily removing the first 2)

It allows to have an extra column with a "AND" type feature for example..

`tf.feature_column.crossed_column([dayfOfWeek, hourOfDay], 24*7)`

It allows the model to realize that journey takes longer and fees are higher at certain DAYxTIME of the week.

#### Bucketizing

How to do feature crosses with "real value column"?

You discretize floats = you "bucketize it! !

It allows to build "groups" of values rather than having continuous all unique values for a feature.

```python
latbuckets = np.linspace(32, 42, lat_nbuckets).tolist()
discrete_lat = tf.feature_column.bucketized_column(lat, latbuckets)
```

#### Model Architecture (Wide and Deep)

Deep learning model (multiple layers) will work well on dense data. e.g. pixel values of an image.
But they will give bad performance on sparse data (when data has many zeros).

On the contrary linear models (1 layer) will be good with sparse data.

```python
tf.estimator.DNNLinearCombinedClassifier(
linear_feature_columns = wide_columns,
dnn_feature_columns = deep_columns,
...
)
```

#### Where to do feature engineering?

3 possible ways:

* on-the-fly, reading the inputs
* as a separate step before training in Dataflow (at scale, in parallel)
* in TensorFlow, only if this processing has to be part of the serving/prediction pipeline. (`tf.transform()` in TensorFlow)


 
----------------------------------------------------------------------------------


## Lab 7: Feature Engineering


In this lab ([overview **video**  of the lab](https://www.coursera.org/learn/serverless-machine-learning-gcp/lecture/nN0uG/feature-engineering-lab-review)), you will perform the following tasks:


* Working with feature columns
* Adding feature crosses in TensorFlow
* Reading much more data from BigQuery
* Creating datasets using Dataflow
* Using a wide-and-deep model


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

### Task 3: Verify that you have a Cloud Storage bucket

This lab notebook requires you to provide a Project ID, Bucket Name, and Bucket Region. If not create them.

```shell
$ echo $DEVSHELL_PROJECT_ID
qwiklabs-gcp-7693c01b6cf22dd2
$
$ echo $BUCKET
$ BUCKET="qwiklabs-gcp-7693c01b6cf22dd2"
$ echo $BUCKET
qwiklabs-gcp-7693c01b6cf22dd2
$
```


### Task 4: Run the lab in the notebook

In Cloud Datalab, click on the Home icon, and then navigate to **datalab/training-data-analyst/courses/machine_learning/feateng**
or check [Github to see its content](https://github.com/GoogleCloudPlatform/training-data-analyst/tree/master/courses/machine_learning/feateng).

Open [**cloudmle.ipynb**](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/courses/machine_learning/feateng/feateng.ipynb).

Read the narrative and execute each cell in turn.


Among the possible improvement other than increasing the amount of training data, we can wonder how feature engineering would impact our inputs:

* Can we use them?
* if No, why?
* if Yes, shall we transform them?

1. Trip distance/duration: NO, not knowable at prediction time
1. Timestamp: NO, not directly. we have to pull out DAY-OF-WEEK and HOUR-OF-DAY. At prediction time we only know the pick-up time, but not the drop-off time
1. HOURS-OF-DAY: YES. Maybe better to quantize it into bins: 'morning rush hours', 'mid-day', evening rush hours', 'night', then one-hot encoding.
1. DAY-OF-WEEK: YES, with one-hot encoding + add weekend versus weekday info + New York holidays if you can find it.
1. lat/long: Yes. we need to for precise distances, but not useful in isolation. we can build new features from it: 
    * binning by neighborhood
    * polygon boundaries
    * proximity to specific bridges/highways
    * sid eof the street for "one way", ...
    * and far simpler... bucketize it by lat/long + feature_cross which is putting lat/long points into grids cells

again, check the resulting [notebook](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/courses/machine_learning/feateng/feateng.ipynb).

Since we do more now on the features using much more data from BigQuery:

* we have our own function `create_query` to pull 'dayofweek', 'hourofday', 'pickuplat', 'pickuplon', the true value which is the 'fare_amount',
* then finally use different modulo to generate the training/validation/testing sets.
* then convert the data from BigQuery into csv using Dataflow (`preprocess()`: convert day of week into strings, convert into csv line and write it down.) You could read from BigQuery and avoid converting it to CSV, but it will be cheaper as you might use the data multiple times, so reading from BigQuery only once.


The python module is now different (in `feaateng/taxifare/trainer`): 

* `task.py` is the same as before, with a single addition: the number of buckets, for hyper-parameter tuning of the number of buckets (grid of NYC).
* in `model.py`:
    * `CSV_COLUMNS` now includes many more feature names with defaults values `DEFAULTS` (even if not used like `sun` for the 'dayofweek', this tells TF t is a string).
    * `SCALE_COLUMNS` are the columns we want to scale
    * `INPUT_COLUMNS` builds the inputs for the neural network in TF.
    * bucketizing lat/long

```python
    # Bucketize the lats & lons
    latbuckets = np.linspace(38.0, 42.0, nbuckets).tolist()
    lonbuckets = np.linspace(-76.0, -72.0, nbuckets).tolist()
    b_plat = tf.feature_column.bucketized_column(plat, latbuckets)
    b_dlat = tf.feature_column.bucketized_column(dlat, latbuckets)
    b_plon = tf.feature_column.bucketized_column(plon, lonbuckets)
    b_dlon = tf.feature_column.bucketized_column(dlon, lonbuckets)
```

    * feature-cross of the pickup-lat and picukup-lon into pickup-location and dropoff-location, make a kind of 'trip' pair out of these (this builds a grid out of the map of new york):

```python
    # Feature cross
    ploc = tf.feature_column.crossed_column([b_plat, b_plon], nbuckets * nbuckets)
    dloc = tf.feature_column.crossed_column([b_dlat, b_dlon], nbuckets * nbuckets)
    pd_pair = tf.feature_column.crossed_column([ploc, dloc], nbuckets ** 4 )
```

    * feature-cross of the hourOfDay and DayOfWeek:

```python
    day_hr =  tf.feature_column.crossed_column([dayofweek, hourofday], 24 * 7)
```

    * wide columns and deep columns: dee columns for all values that are real-valued numbers. And also "embed" some of those features into a group of 10 numbers out of it, and use it as my input to the neural network.


```python
   # Wide columns and deep columns.
    wide_columns = [
        # Feature crosses
        dloc, ploc, pd_pair,
        day_hr,

        # Sparse columns
        dayofweek, hourofday,

        # Anything with a linear relationship
        pcount 
    ]

    deep_columns = [
        # Embedding_column to "group" together ...
        tf.feature_column.embedding_column(pd_pair, 10),
        tf.feature_column.embedding_column(day_hr, 10),

        # Numeric columns
        plat, plon, dlat, dlon,
        latdiff, londiff, euclidean
    ]
```



## Hyperparameter Tuning

[demo notebook](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/courses/machine_learning/feateng/hyperparam.ipynb)

**Learning rate** and **batch size** matter.

* at low learning rate, the improvement is linear, but not best possible performance
* at high learning rate, the improvement is faster, but you tend to never converge

Often there is "just right learning rate" but good luck finding it.

Same for batch size and other parameters (size of model, number of hask buckets, embedding size, ...)

Cloud MLE hyperparameter tuning optimizes those for you. For that you need to:

1. make the parameter a command-line argument (in `task.py`)
2. make sure outputs don't clobber each other (with the `trial` variable in `task.py`)
3. supply hyperparameters to your training job (with `hyperparam.yaml`)

Cloud MLE will search for promising hyperparameters if you pass the config file with `--config=hyperparam.yaml` when you submit your training job using the command line (`gcloud ml-engine jobs submit training $JOBNAME .... --config=hyperparam.yaml ...`).


We tell it that our goal is to minimize the RMSE (`eval_metrics` in `modle.py`), and give the tuner ranges for each of the command line parameters we want to use. It is not Grid Search, but a Bayseian search.
 







* [Exploring `tf.transform`](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/courses/machine_learning/feateng/tftransform.ipynb) for data processing of input data in prediction.


## Moral of the taxi fare use case

**Accuracy** improves with:

* **feature engineering**,
* **hyperparameter tuning**,
* and **using lots of data**.


## ML abstraction

You could use already made professional ML API to solve your problem. Among them:

* NLP API
* Vision API
* Dialogflow

**Cloud autoML** automatically create ML models:

* it provides a GUI that streamline the process of working with data
* after training you can create your own label on top of the labels already recognized by autoML.

It works by steps, e.g.:

1. step 1: upload the images
1. step 2: train (the models)
1. step 3: evaluate and serve the model in an application.





----------------------------------------------------------------------------------

## Other references


* [Angeliqa's notes](https://docs.google.com/document/d/1_G6F5qmjcklNOK8KBkUBD5gDHMLXrNjCX6yIBuR8iEA/edit?pli=1)
* [GCP Labs and demos for the course](https://github.com/GoogleCloudPlatform/training-data-analyst)
* [Cloud Datalab resources on Github](https://github.com/googledatalab/notebooks)
* [Blog on Big Data + ML](https://cloud.google.com/blog/products/data-analytics) (e.g. [Empowering nonprofits through large-scale analytics](https://blog.google/products/google-cloud/data-solutions-for-change-empowering-nonprofits-through-large-scale-analytics/))

----------------------------------------------------------------------------------

## Recommended extra courses

* [Machine Learning with TensorFlow on GCP **Specialization**](https://www.coursera.org/specializations/machine-learning-tensorflow-gcp) (5 courses, 7+ weeks)
    * [course 1: How Google does Machine Learning](https://www.coursera.org/learn/google-machine-learning)
    * [course 2: Launching into Machine Learning](https://www.coursera.org/learn/launching-machine-learning)
    * [course 3: Intro to TensorFlow](https://www.coursera.org/learn/intro-tensorflow)
    * [course 4: Feature Engineering](https://www.coursera.org/learn/feature-engineering)
    * [course 5: Art and Science of Machine Learning](https://www.coursera.org/learn/art-science-ml)
* https://ai.google/education/
* https://developers.google.com/machine-learning/crash-course/
