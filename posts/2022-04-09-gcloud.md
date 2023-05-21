---
aliases:
- /markdown/2022/04/09/gcloud
categories:
- Gcloud
date: '2022-04-09'
author: Zeel B Patel
description: Most used commands while working with gcloud
layout: post
title: Gcloud cheatsheet
toc: true

---

## Initial setup
Following [this guide](https://cloud.google.com/tpu/docs/run-calculation-jax).

* To set default email, project-id & zone:
```
gcloud config set account your-email-account
gcloud config set project your-project-id
gcloud config set compute/zone us-central1-f  # us-central1-f for free v2 TPUs and europe-west4-a for free v3 TPUs (only if you have free TRC access)
```
* To get currently active project and zone related info:
```
gcloud info
```
* To create an identity (I don't know if this is required or not. This command should trigger installation of "gcloud Beta Commands" automatically in another shell and then you need to rerun the following command):
```
gcloud beta services identity create --service tpu.googleapis.com
```

## Working with TPU VMs
There are two different terms here: "TPU VMs" and "TPU nodes". TPU nodes can be connected externally via another VM. TPU VMs are stand-alone systems with TPUs, RAM and CPU (96 core Intel 2 GHz processor and 335 GB RAM). We may be charged via GCP for the VM (CPUs and RAM). (I will update this info once I know for sure):

<img width="512" alt="image" src="https://user-images.githubusercontent.com/59758528/162559104-fadd6d54-c2ec-4117-8d92-2094643c46f6.png">

* To create a TPU VM in preferred zone via CLI (be careful about the `--zone` to avoid charges, check the first email received from TRC team to see what kind of TPUs are free in different zones. if `--zone` is not passed, VM will be created in the default zone that we set initially. This command triggered installation of "gcloud Alpha Commands"):
```
gcloud alpha compute tpus tpu-vm create vm-1 --accelerator-type v2-8 --version tpu-vm-tf-2.8.0 --zone us-central1-f
```
* To get the list of TPU nodes/VMs:
```
gcloud compute tpus list
```
* To delete a TPU node/VM:
```
gcloud compute tpus delete vm-1
```
* To connect with a vm via ssh (this automatically creates ssh key pair and places in default ssh config location):
```
gcloud alpha compute tpus tpu-vm ssh vm-1
```
* [Follow this guide to create and attach a persistent disk with the TPU VM](https://cloud.google.com/tpu/docs/setup-persistent-disk)

## Working with TPU VMs via VS-code
* Install the following extension on VS-code:
<img width="734" alt="image" src="https://user-images.githubusercontent.com/59758528/162790778-3ca244d1-fe2f-47aa-89ab-d82861ebd9af.png">

* Use the following button to connect to a remote machine (use "Connect to Host..." button):
<img width="350" alt="image" src="https://user-images.githubusercontent.com/59758528/162791286-ca780943-e8d5-4619-91b5-0978743141f5.png">


* Manually update the default ssh config file (in my case, "C:\Users\zeelp\\.ssh") to add a VM in VS-code (you can use VS-code command palette to figure out the config file for you and edit it. Please see the screeshot below). 

<img width="457" alt="image" src="https://user-images.githubusercontent.com/59758528/162791831-d18031be-9714-4857-afad-ad809bed701e.png">

* Note that ssh public-private key pair with name `google_compute_engine` is automatically generated when you connect with the VM for the first time with `gcloud alpha compute tpus tpu-vm ssh` command. The VM config for me looks like this:

```
Host Cloud-TPU-Node-2
  HostName <External-IP-of-your-TPU-VM>
  User zeelp
  Port 22
  IdentityFile C:\Users\zeelp\.ssh\google_compute_engine
```
