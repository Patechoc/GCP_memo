# Google Cloud Shell & gcloud


Google Cloud Shell provides: **command-line access** to computing resources hosted **on Google Cloud Platform** and is available in the Google Cloud Platform Console. 

It makes it easy for you to manage your projects and resources without having to install the Google Cloud SDK and other tools on your laptop. 

With Cloud Shell, the **Cloud SDK `gcloud` commands** and other utilities you need are always available when you need them.


## Setup and Requirements

* Confirm that you are authenticated

```
gcloud auth list
```

### Show & active your project

```
gcloud config list project
```

if not, you can set it:

```
gcloud config set project <PROJECT_ID>
```

or 

`export PROJECT_ID=$(gcloud info --format='value(config.project)')`

If you already have selected your project before opening CloudShell.

### Set up your zone

`gcloud config set compute/zone us-west1-b`

## First command line

After Cloud Shell launches, you can use the command line to invoke the Cloud SDK gcloud command or other tools available on the virtual machine instance.

> You can also use your `$HOME` directory in persistent disk storage to store files across projects and between Cloud Shell sessions. Your `$HOME` directory is private to you and cannot be accessed by other users.

* `gcloud -h`
* `gcloud config --help` or `gcloud help config`
* `gcloud config list` or more detailled like `gcloud config list --all`

## Learn More

Cloud Shell Documentation (https://cloud.google.com/shell/docs/) and [tutorial video](https://www.youtube.com/watch?v=hBMcAKzGt3w).
`gcloud` Documentation (https://cloud.google.com/sdk/gcloud/) and [tutorial video](https://www.youtube.com/watch?v=oTIK9OvQBxQ&list=PLIivdWyY5sqIij_cgINUHZDMnGjVx3rxi&index=15).

## References

* https://cloud.google.com/sdk/gcloud/
* https://cloud.google.com/cloud-shell/docs/quickstart
* https://cloud.google.com/sdk/gcloud/
