# k8s-boinc

Full implementation of Boinc in Kubernetes.  It includes examples of physical volumes, secrets, and resource limits as well. 

This manifest creates a single pod that does the work.  It contains a NFS definition for persistence.

## Prerequisites

* You will need a running Kubernetes cluster, and access to the control plane.  I am running v1.18 on five work nodes and one master.
* You will need to set up a NFS share to contain your persistent project data.

## Setup

Simply edit the YAML file, and make changes as needed.  

### Things that have to change are:
* PersistentVolume: Change the persistent volume definition.  I was using a local NFS share. 
* Secret: I set the Boinc client password as a secret rather than hard-coding it as a ConfigMap entry.  The command is:
    $ echo -n "MyPassword" | base64
    TXlQYXNzd29yZA==

### Things that are optional:
* Namespace: I create a namespace called "boinc" for this.  Change it to whatever you like.
* ResourceQuota: Feel free to delete this if you don't need it.  Also delete the Resource definitions in the Deployment.
* ConfigMap: Change the `timezone: "America/Chicago"` line to your local timezone.
* Deployment: I set this as a `type: NodePort` and set the client IP as the port so that I can access it on that port from either the master server or any of the nodes.
