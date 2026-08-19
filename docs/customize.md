## Customize Cluster

Now that we've verified that we can connect to the cluster via OpenOnDemand, let's customize it. First, let's terminate both the OpenOnDemand instance and the Slurm cluster from CycleCloud by clicking on the "Terminate" button. 

## Add new cluster-init project

Once the cluster has been terminated, ssh into the CycleCloud VM and clone this repo.

```bash
git clone [url to clone this repo]
```

Once the directory has been cloned, go to the cvmfs-eessi directory under projects: 

```bash
cd ccws-learn/projects/cvmfs-eessi
```

and upload the project to CycleCloud:

```bash
sudo cyclecloud project upload azure-storage
```

## Apply new cluster-init project
With this new cluster-init project, we can configure our cluster to stream software from the EESSI repository. From the CycleCloud portal page, got to the advanced settings and add the cvmss-eessi:default:1.0.0 cluster-init project for the login and compute nodes. 

## Configure visualization nodes
Modify the htc node array configuration to use the Cendio Thinlinc image, by selecting the "Custom Image" checkbox and entering the text below as the custom image version to target: 

```
cendio:thinlinc:thinlinc-ubuntu-2204:latest
```
## Validate Slurm Settings

At the very top of the advanced settings page, you will find the slurm settings. Validate that in the drop-down for `SSL Certificate URL`, a value is selected. If it is empty, click on the drop down and select `AzureCA.pem`. 

## Update Required Settings
Finally, in the auto-scaling section, uncheck `Specify availability zones for nodes`. 

## Start Cluster

From the CycleCloud portal re-start the cluster by clicking on the play button to start the cluster. Start up the Slurm cluster first and then the OpenOnDemand instance so that it can automatically detect the instantiated cluster. 

As you wait for the cluster to start, download and install the Thinlinc client from the Cendio [website](https://www.cendio.com/thinlinc/download/) in your deployer instance. We will use the client  to connect to VDI instances in the next exercise. 

[Next Step: Run an HPC Job ](./openfoam.md)
