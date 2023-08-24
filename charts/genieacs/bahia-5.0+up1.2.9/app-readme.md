# GenieACS

[GenieACS](https://genieacs.com/) is an open source TR-069 remote management solution with advanced device provisioning capabilities

For more detailed documentation please visit [this page](http://docs.genieacs.com/en/latest/)

Introduction
------------

This chart installs GenieACS on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

Prerequisites
--------------------

1. An external Load Balancer or metalLB as LoadBalancer controller.
2. An ingress controller running in your cluster (e.g. ingress-nginx). Make sure it's exposed as an ExternalIP or LoadBalancer service.
3. At least 2 available LoadBalancer IP addresses for CWMP and FS services.
4. A default Storage Class, and its corresponding storage provider (e.g. OpenEBS). 
5. Mongodb community operator, which will create and manage the mongo database.
6. Optionally a genieACS tester running outside the cluster.

Pre-installation
-----------------

You can install both the **Mongodb community operator** (mandatory) and **OpenEBS** (optional) from the Marketplace with few changes to default values.

- **Mongodb community operator**: set `operator.watchNamespace: '*'` in YAML values. Install in a dedicated namespace.

- **OpenEBS**: In **NDM Sparse Disk Settings**, disable `Create a cStor Pool on Sparse Disks` checkbox. Install in any namespace.

Don't forget to set `openebs-hostpath` as the default Storage Class. Go to Storage -> StorageClasses -> Click on the 3 dots -> Click on *Set as default*  

Installation
-----------------

1. Click on the **Install** button. 
2. fill in the form with the required parameters and click on **Install**. For a simple installation YAML customization is not required.

After the installation you will see 4 deployments and 4 services running. UI and NBI will be exposed through an ingress, CWMP and FS through Load Balancer IP addresses.

In order to test the installation, execute the following steps:

1. Go to the URL you set as the UI, check that the page loads and bootstraps the application succesfully
2. Launch the genieACS tester with the following command from any external host that can reach CWMP Load Balancer IP: 
    ```bash
    docker run -d --name genieacs-sim --network host drumsergio/genieacs-sim ./genieacs-sim -u http://<CWMP Load Balancer IP>:7547
    ```
3. Go to the UI and click on the Devices tab, then click in **Show**. You should see the device status and parameters.
4. In the UI, go to the Admin tab -> Files -> New , and upload any file (e.g. test.txt). Set the OUI, Product Class and Version according to the genieACS tester parameters (device tab). 
5. Click on **save**
6. From any external host that can reach the UI URL, execute the following curl command:
    ```bash
     curl -i 'http://<UI URL>/devices/<device ID>/tasks?timeout=9000&connection_request' -X POST --data '{"name": "download", "file": "test.txt"}'
    ```
   <Device ID> can be found in the last section of the URL in step 3.
7. Check that there are no errors in the Device section.