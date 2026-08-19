## Connect to Cluster

Go to the azure portal to find the private IP address of the cyclecloud VM. Copy the private ip address so that you can ssh to the CycleCloud VM: 

```bash
ssh [private ip of cyclecloud vm]
```

From the cyclecloud VM, you can monitor the progress of the CycleCloud setup by tailing the log of cloud-init: 

```bash
tail -f /var/log/cloud-init-output.log
```

Once you see that the installation has completed, you can verify that the cluster is now starting by running the following command as root: 

```bash
cyclecloud show_cluster ccw
```

You should see that the cluster is in the **started** state. 

## Connect to CycleCloud Admin Portal

Open your browser and go connect to the CycleCloud portal:
```bash
https://[private ip of CycleCloud VM]
```
From the admin portal you can view the state of the cluster and monitor the startup. Additionally, you can make changes to the configuration of the cluster. 

## Connect to Cluster via Open OnDemand

To access the cluster, you will need to log in to Open Ondemand using Entra Authentication via OIDC: 

```bash 
https://[private ip address of Open OnDemand VM]
```

[Next Step: Customize ](./customize.md)



