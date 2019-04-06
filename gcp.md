---
layout: page
title: Google Cloud Platform Setup
description: Guides to setup Google Cloud Platform (GCP) for course homework and project.
---

## Before You Start
---

Use a non-Stanford Google account.  
Redeem your GCP credits provided by the course.  
Google and Piazza are always helpful.

## Prepare your Google Cloud Platform account
---

### Create a new project named cme-213 (or any name you prefer)

You can create and manage your GCP projects on the [Resource Management Page](https://console.cloud.google.com/cloud-resource-manager).
### [hw3 and after] Apply for GPU quotas

You can request GPU quotas on the [Quotas Page](https://console.cloud.google.com/iam-admin/quotas).
See [guide](https://cloud.google.com/compute/quotas) provided by Google if you run into any difficulties.

Two types of quotas are needed throughout the course:
* `GPUs (all regions)`
* `Nvidia K80 GPUs` at location `us-west1`

You will need at least 4 for both quotas. You will not be charged for requesting quotas.
It can take up to 48 hours to process your request, so we suggest requesting your quotas as early as possible.

## Install Google Cloud SDK on your local machine
---
Google Cloud SDK (`gcloud`) is a software package that allows you to manage your GCP projects from your local machine through terminals.
We are going to use `gcloud` to run scripts that will create the correct virtual machines for you.

`gcloud` can be installed on a variety of operating systems including macOS and Windows.  
Installation procedures can be found [here](https://cloud.google.com/sdk/docs/downloads-interactive).

## Manage virtual machine
---

You should use separate virtual machines for different homework and project. Environment is setup for each virtual machine by the script we provide.

### Create your virtual machine
1. Start a terminal
2. `cd` to your starter code directory 
3. Run `./create_vm.sh`

You will see something like this if success
```
Updated property [compute/zone].
Created [https://www.googleapis.com/compute/v1/projects/cme-213/zones/us-west1-b/instances/hw2].
NAME  ZONE        MACHINE_TYPE  PREEMPTIBLE  INTERNAL_IP  EXTERNAL_IP    STATUS
hw2   us-west1-b  n1-highcpu-8               10.138.0.24  35.197.115.51  RUNNING
Installing necessary libraries. You will be able to log into the VM after several minutes with:
gcloud compute ssh hw2
```

You should remember the name of your VM (`hw2` in this case) and use it for `NAME` in the commands below.

**Billing starts now!!!**

### Start your virtual machine
Your virtual machine should start automatically after you created it. However you will need to manually start your machine if you stopped it.  

To start a virtual machine:
1. Start a terminal
2. Run `gcloud compute instances start NAME`

or through [Compute Page](https://console.cloud.google.com/compute/).

**Billing starts now!!!**

### Log into your virtual machine
You can only log into your VM after it is started.

To log into your virtual machine
1. Start a terminal
2. Run `gcloud compute ssh NAME`

or through [Compute Page](https://console.cloud.google.com/compute/).

You might need to wait for a while if you see the following message. It's likely we are installing necessary packages for the homework/project and disabled ssh.
```
ssh: connect to host 35.197.115.51 port 22: Connection refused
ERROR: (gcloud.compute.ssh) [/usr/bin/ssh] exited with return code [255].
```

### Stop your virtual machine
We manage to setup an auto-shutdown service on your VMs through `create_vm.sh`. It will automatically stop your VM after 30 minutes of disconnection. However
it won't work if you use tools that keep you logged in, like `tmux` or `screen`. So
**Stop your VM when you are not using it!!!**

To stop your virtual machine
1. Start a terminal
2. Run `gcloud compute instances stop NAME`

or through [Compute Page](https://console.cloud.google.com/compute/).

### Delete your virtual machine
Your don't usually need to delete virtual machines --- you only need to stop them. However if you mess things up you can always start over by deleting and recreating your virtual machine.

To delete your virtual machine
1. Start a terminal
2. Run `gcloud compute instances delete NAME`

or through [Compute Page](https://console.cloud.google.com/compute/).

### Transfer files between local machine and VMs
It is recommended to use `scp` to transfer files to/from your virtual machine:

To transfer to your VM:
1. Start a terminal
2. Run `gcloud compute scp LOCAL_PATH NAME:VM_PATH`, where `LOCAL_PATH` is the path to your local file and `VM_PATH` is the VM destination your file will appear

To transfer from your VM:
1. Start a terminal
2. Run `gcloud compute scp NAME:VM_PATH LOCAL_PATH`, where `VM_PATH` is the path of file on your VM and  `LOCAL_PATH` is the local destination

You can use `--recurse` flag to transfer a directory. For example:  
`gcloud compute scp --recurse ./starter_code hw2:~/`  
will transfer the local `starter_code` folder to your hw2 virtual machine.


### Other helpful commands
`gcloud` is powerful and allows you to do about anything about your cloud platform. Here are some useful commands:
* `gcloud compute instances list` will list all of your VMs
* `gcloud compute instances reset NAME` will reset your VM (should be equivalent as delete then create)
* `gcloud compute --help` will show help for `gcloud compute`

Remember you can always manage your VMs from the [Compute Page](https://console.cloud.google.com/compute/) if you are not comfortable with command line tools.

### Keep a mind on your credits
You can see how you are doing with your credits on the [Billing Page](https://console.cloud.google.com/billing/).

Here we provide a table of specs for each type of virtual machine used for homework and project:

|           | CPU cores | Memory     | Disk        | GPU type    | GPU number | Hourly cost |
| --------- | :-------: | :--------: | :---------: | :---------: | :--------: | :---------: |
| HW2       | 8         | 7.2GB      | 10GB        | -           | -          | $0.199      |

