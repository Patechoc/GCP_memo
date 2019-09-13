# GCP Fundamentals: Getting Started with App Engine

[video](https://www.coursera.org/learn/gcp-fundamentals/lecture/T1C9J/demonstration-getting-started-with-app-engine)

~ 20 minutes

In this lab, you create a simple App Engine application using the Cloud Shell local development environment, and then deploy it to App Engine.

## Objectives

In this lab, you learn how to perform the following tasks:

Preview an App Engine application using Cloud Shell.

Launch an App Engine application.

Disable an App Engine application.

## Task 1: Sign in to the Google Cloud Platform (GCP) Console

Before you click the Start Lab button
Read these instructions. Labs are timed and you cannot pause them. The timer, which starts when you click Start Lab, shows how long Cloud resources will be made available to you.

This Qwiklabs hands-on lab lets you do the lab activities yourself in a real cloud environment, not in a simulation or demo environment. It does so by giving you new, temporary credentials that you use to sign in and access the Google Cloud Platform for the duration of the lab.

## Task 2: Preview an App Engine application

On the Google Cloud Platform menu, click Activate Cloud Shell Activate Cloud Shell. If a dialog box appears, click Start Cloud Shell.

Clone the source code repository for a sample application called guestbook:

`git clone https://github.com/GoogleCloudPlatform/appengine-guestbook-python`

Navigate to the source directory:

`cd appengine-guestbook-python`

List the contents of the directory:

`ls -l`

View the app.yaml file and note its structure:

`cat app.yaml`

YAML is a templating language. YAML files are used for configuration of many Google Cloud Platform services, although the valid objects and specific properties vary with the service. This file is an App Engine YAML file with handlers: and libraries:. A Cloud Deployment Manager YAML file, for example, would have different objects.

Run the application using the built-in App Engine development server.

`dev_appserver.py ./app.yaml`

The App Engine development server is now running the guestbook application in the local Cloud Shell. It is using other development tools, including a local simulation of Datastore.

In Cloud Shell, click Web preview Web Preview > Preview on port 8080 to preview the application.

To access the Web preview icon, you may need to collapse the Navigation menu.


Try the application. Make a few entries in Guestbook, and click Sign Guestbook after each entry.

Using the Google Cloud Platform Console, verify that the app is not deployed. In the GCP Console, on the Navigation menu (Navigation menu), click App Engine > Dashboard. Notice that no resources are deployed. The App Engine development environment is local.

To end the test, return to Cloud Shell and press `Ctrl+C` to abort the App Engine development server.

## Task 3: Deploy the Guestbook application to App Engine

Ensure that you are at the Cloud Shell command prompt.

Deploy the application to App Engine using this command:

`gcloud app deploy ./index.yaml ./app.yaml`

If prompted for a region, enter the number corresponding to the region that Qwiklabs or your instructor assigned you to. Type Y to continue.

To view the startup of the application, in the GCP Console, on the Navigation menu (Navigation menu), click App Engine > Dashboard.

You may see messages about "Create Application". Keep refreshing the page periodically until the application is deployed.

View the application on the Internet. The URL for your application is https://PROJECT_ID.appspot.com/ where PROJECT_ID represents your Google Cloud Platform project name. This URL is listed in two places:

- The output of the deploy command: Deployed service [default] to [https://PROJECT_ID.appspot.com]
- The upper-right pane of the App Engine Dashboard
- Copy and paste the URL into a new browser window.

You may see an INTERNAL SERVER ERROR. If you read to the bottom of the page, you will see that the error is caused because the Datastore Index is not yet ready. This is a transient error. It takes some time for Datastore to prepare and begin serving the Index for guestbook. After a few minutes, you will be able to refresh the page and see the guestbook application interface.


Congratulations! You created your first application using App Engine, including exercising the local development environment and deploying it. It is now available on the internet for all users.

- Click Check my progress to verify the objective.
- Deploy the Guestbook application to App Engine

## Task 4: Disable the application

App Engine offers no option to undeploy an application. After an application is deployed, it remains deployed, although you could instead replace the application with a simple page that says something like "not in service."

However, you can disable the application, which causes it to no longer be accessible to users.

In the GCP Console, on the Navigation menu (Navigation menu), click App Engine > Settings.

Click Disable application.

Read the dialog message. Enter the App ID and click DISABLE.

If you refresh the browser window you used to view to the application site, you'll get a 404 error.


## Congratulations!

In this lab, you deployed an application on App Engine.

## More resources

Read a [detailed explanation of this example App Engine application](https://cloud.google.com/appengine/docs/standard/python/getting-started/creating-guestbook).