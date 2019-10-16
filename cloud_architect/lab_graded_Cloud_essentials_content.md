# PCA Prep - Google Cloud Essential Skills: Challenge Lab


~45 minutes

## Content

* [Overview](#overview)
* [Setup](#setup)
   * [Before you click the Start Lab button](#before-you-click-the-start-lab-button)
   * [What you need](#what-you-need)
* [Challenge scenario](#challenge-scenario)
* [Your challenge](#your-challenge)
   * [Running a Basic Apache Web Server](#running-a-basic-apache-web-server)
      * [Create a Linux VM Instance](#create-a-linux-vm-instance)
      * [Enable Public Access to VM Instance](#enable-public-access-to-vm-instance)
      * [Running a Basic Apache Web Server](#running-a-basic-apache-web-server-1)
      * [Test Your Server](#test-your-server)
      * [Troubleshooting](#troubleshooting)
* [Congratulations!](#congratulations)


## Overview

This lab is similar to a Challenge Lab in the "Challenge: GCP Architecture" Quest.

For this Challenge Lab, you must complete a series of tasks within a limited time period. Instead of following step-by-step instructions, you'll be given a scenario and task - you figure out how to complete it on your own! An automated scoring system (shown on this page) will provide feedback on whether you have completed your tasks correctly.

To score 100% you must complete all tasks within the time period!

When you take a Challenge Lab, you will not be taught GCP concepts. You'll need to use your advanced Google Compute Engine (GCE) skills to assess how to build the solution to the challenge presented. This lab is only recommended for students who have GCE skills. Are you up for the challenge?

Topics tested

* Create a Linux Virtual Machine Instance
* Enable public access to VM instance
* Running basic Apache Web Server
* Test your server

## Setup

### Before you click the Start Lab button

Read these instructions. Labs are timed and you cannot pause them. The timer, which starts when you click Start Lab, shows how long Cloud resources will be made available to you.

This Qwiklabs hands-on lab lets you do the lab activities yourself in a real cloud environment, not in a simulation or demo environment. It does so by giving you new, temporary credentials that you use to sign in and access the Google Cloud Platform for the duration of the lab.

### What you need

To complete this lab, you need:

* Access to a standard internet browser (Chrome browser recommended).
* Time to complete the lab.

**Note**: If you already have your own personal GCP account or project, do not use it for this lab.


## Challenge scenario

Your company is ready to launch a brand new product! Because you are entering a totally new space, you have decided to deploy a new website as part of the product launch. The new site is complete, but the person who built the new site left the company before they could deploy it.

## Your challenge

Your challenge is to deploy the site in the public cloud by completing the tasks below. You will use a simple Apache web server as a placeholder for the new site in this exercise. Good luck!

### Running a Basic Apache Web Server

A virtual machine instance on Google Compute Engine can be controlled like any standard Linux server. Deploy a simple Apache web server (a placeholder for the new product site) to learn the basics of running a server on a virtual machine instance.

#### Create a Linux VM Instance

Create a Linux virtual machine, name it apache and specify the zone as us-central1-a.

#### Enable Public Access to VM Instance

While creating the Linux instance, make sure to apply the appropriate firewall rules so that potential customers can find your new product.

#### Running a Basic Apache Web Server

A virtual machine instance on Google Compute Engine can be controlled like any standard Linux server. Deploy a simple Apache web server (a placeholder for the new product site) to learn the basics of running a server on a virtual machine instance.

#### Test Your Server

Test that your instance is serving traffic on its external IP. You should see the "Apache default" page (a placeholder for the new product site).

#### Troubleshooting

* Receiving a Connection Refused Error:

    * Your VM instance is not publicly accessible because the VM instance does not have the proper tag that allows Compute Engine to apply the appropriate firewall rules, or your project does not have a firewall rule that allows traffic to your instance's external IP address.
    * You are trying to access the VM using an https address. Check that your URL is http://EXTERNAL_IP and not https://EXTERNAL_IP


## Congratulations!

You have completed the challenge lab.
