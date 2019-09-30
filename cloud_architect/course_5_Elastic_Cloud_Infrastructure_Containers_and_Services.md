# Elastic Cloud Infrastructure: Containers and Services

## Content

ToC


## Course Intro

- course site: https://www.coursera.org/learn/gcp-infrastructure-containers-services
- [1-week course intro video](https://www.coursera.org/learn/gcp-infrastructure-containers-services/lecture/sIb77/elastic-cloud-infrastructure-containers-and-services-course-intro): Elastic Cloud Infrastructure: Containers and Services


- [Course overview video](https://www.coursera.org/learn/gcp-infrastructure-containers-services/lecture/OEhAf/module-overview-intro)

**What does "Containers and Services" mean?**

Well, let's start with **containers**. Essentially, containerization is something that Google's been doing for the last 12 years. It's essentially that hybrid between virtual machines and pure Platform as a Service. Now, you can deploy your code in a very nice little package with its dependencies. But the great thing is, it's portable. It can run on-premise, it can run in other cloud providers. Especially, it runs really well in Google Cloud Platform.

So, with containerization in general, we're going to introduce something we call **Kubernetes**, which is essentially **an outsourced version of** the internal system we call **Borg**. Borg essentially finds a home for your containers to live. Kubernetes helps you to automate and orchestrate where your containers live. If you want to take it even further, you can install Kubernetes, you can go to kubernetes.io. It's an open-source project. You can install that on-prem, or other individual cloud providers as well.

We have **a managed service version of that**, and that's called **Google Kubernetes Engine**. So now, we've automated a lot of that infrastructure process, but we've added some additional enhancements. Since Kubernetes has to run anywhere, it can't really integrate as well with native cloud provider features. Now, if you took some of our other courses, you may have learned about autoscaling, our firewall rules, load balancers, et cetera. Google Kubernetes Engine integrates into all those, and so it takes advantage of our global network. It can set up the firewall rules for you, load balancing, autoscaling rules, and you're also doing this with containers. So you really get a huge benefit running on Google Container Engine.

Now, then, **what about our services?** What are they? Well, first of all, they could be the application infrastructure.

- This could be our **Platform as a Service**. These are where you're deploying your own code Java, Python, Go, PHP, you name it.
- Perhaps you want to take advantage of **micro services** so you have **cloud functions**. We also have **Cloud Endpoints**, in which you can publish your APIs. And plus, there's a number of other managed services. Google Cloud Pub/Sub for messaging services between your applications, and thousands of individual APIs that are available to you and your applications, such as Google Maps API, Google Analytics, being able to pull calendar events from Google Calendar and, of course, a number of different cloud APIs, from machine learning APIs, vision APIs, sound, et cetera. You can go ahead and use our API Explorer to find out more, but there's going to be a great deal covered inside of this individual course.


## Module overview

- [module review video](https://www.coursera.org/learn/gcp-infrastructure-containers-services/lecture/L778e/module-1-review-outro)

<img src="../images/application_infrastructure_services.png"
     alt="application_infrastructure_services.png"
     style="float: left; margin-right: 10px;" />

This module covers **"Application Infrastruture Services Concepts"**:

* Cloud Pub/Sub
* API Management
* Cloud Functions
* Cloud Source Repositories
* Specialty APIs

## Cloud Pub/Sub

[video](https://www.coursera.org/learn/gcp-infrastructure-containers-services/lecture/oiW44/cloud-pub-sub)



<img src="../images/pubsub.png"
     alt="pubsub.png"
     style="float: left; margin-right: 10px;" />

### Benefits of Cloud Pub/Sub

<img src="../images/pubsub_benefits.png"
     alt="pubsub_benefits.png"
     style="float: left; margin-right: 10px;" />

### Basics of Pub/Sub

<img src="../images/pubsub_basics.png"
     alt="pubsub_basics.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/pubsub_basics_multiple_publishers_subscribers.png"
     alt="pubsub_basics_multiple_publishers_subscribers.png"
     style="float: left; margin-right: 10px;" />

### Complexity handled by Pub/Sub 

<img src="../images/pubsub_basics_complexity.png"
     alt="pubsub_basics_complexity.png"
     style="float: left; margin-right: 10px;" />

### Integrations for publishing or subsribing

<img src="../images/pubsub_integration_tools.png"
     alt="pubsub_integration_tools.png"
     style="float: left; margin-right: 10px;" />

### Use cases for Pub/Sub

<img src="../images/pubsub_use_cases.png"
     alt="pubsub_use_cases.png"
     style="float: left; margin-right: 10px;" />


## API Management
 
[video](https://www.coursera.org/learn/gcp-infrastructure-containers-services/lecture/SA2Uc/api-management)

### Cloud Endpoints

<img src="../images/API_Management_Cloud_Endpoints.png"
     alt="API_Management_Cloud_Endpoints.png"
     style="float: left; margin-right: 10px;" />

Example: 

<img src="../images/API_Management_Cloud_Endpoints_example.png.png"
     alt="API_Management_Cloud_Endpoints_example.png.png"
     style="float: left; margin-right: 10px;" />

### Apigee

<img src="../images/API_Management_Apigee.png.png"
     alt="API_Management_Apigee.png.png"
     style="float: left; margin-right: 10px;" />



## Cloud Functions: Serverless offering

### A microservices architecture responding on triggers

<img src="../images/Cloud_Functions.png.png"
     alt="Cloud_Functions.png.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/Cloud_Functions_definition.png"
     alt="Cloud_Functions_definition.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/Cloud_Functions_.png"
     alt="Cloud_Functions_.png"
     style="float: left; margin-right: 10px;" />


### Differences Cloud Functions/Endpoints

1. Endpoint(s)
    - Cloud Enpoints exposes an array of endpoints or API functions.
    - Cloud Functions exposes a **single** endpoint
2. Backend
- Cloud Endpoint backend is an AppEngine backend. So you have a **long running programming environment** with full access to complex data, and storage services
- In Cloud Functions, you have one single piece of code that accepts a limited input, executes **rapidly, produces some output, and then exits**.


<img src="../images/Differences_Cloud_Functions_Cloud_Endpoints.png"
     alt="Differences_Cloud_Functions_Cloud_Endpoints.png"
     style="float: left; margin-right: 10px;" />

### Demo: Cloud Functions

[demo video](https://www.coursera.org/learn/gcp-infrastructure-containers-services/lecture/IR61q/demo-cloud-functions)

1. In CloudShell:

- `npm install --save @google-cloud/storage`
- `mkdir ~/gcf_hello_world`

2. Create `index.js`:

```javascript
exports.helloGET = (req, res) => {
    res.send("Hello World");
}
```

3. Deploy it

`gcloud beta functions deploy helloGET --trigger-http`

4. Use the generated https URL to trigger the function



## Cloud Source Repositories

[video](https://www.coursera.org/learn/gcp-infrastructure-containers-services/lecture/vvqmA/cloud-source-repositories)


<img src="../images/Cloud_Source_Repositories.png.png"
     alt="Cloud_Source_Repositories.png.png"
     style="float: left; margin-right: 10px;" />

<img src="../images/Cloud_Source_Repositories_definition.png.png"
     alt="Cloud_Source_Repositories_definition.png.png"
     style="float: left; margin-right: 10px;" />
     
## Specialty APIs

<img src="../images/Specialty_APIs_long_list.png.png"
     alt="Specialty_APIs_long_list.png.png"
     style="float: left; margin-right: 10px;" />

Example: Cloud ML

<img src="../images/Specialty_APIs_long_list_example_Cloud_ML.png.png"
     alt="Specialty_APIs_long_list_example_Cloud_ML.png.png"
     style="float: left; margin-right: 10px;" />



<img src="../images/dummy.png"
     alt="dummy.png"
     style="float: left; margin-right: 10px;" />



<img src="../images/dummy.png"
     alt="dummy.png"
     style="float: left; margin-right: 10px;" />



<img src="../images/dummy.png"
     alt="dummy.png"
     style="float: left; margin-right: 10px;" />







## Resources/Articles

