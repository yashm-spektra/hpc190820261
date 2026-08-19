## Run an HPC Job

After restarting your cluster, connect via Open Ondemand using Entra Authentication via OIDC: 

```bash 
https://[private ip address of Open OnDemand VM]
```
## Prepare job submission

Once you've logged in, validate that you can load the EESSI modules by running these commands: 

```bash 
source /cvmfs/software.eessi.io/versions/2023.06/init/bash 
ml avail 
```

Clone this directory in your login node and navigate to the examples folder: 

```bash
git clone [url to clone this repo]

cd examples
```

## Submit OpenFoam Job
Review the openfoam/submit.slurm file:
```bash
cat openfoam/submit.slurm
```

Submit the job: 
```bash
sbatch -N 1 -p hpc openfoam/submit.slurm 

squeue 
```

Monitor the content of the logs under ~/openfoam_tutorial_runs/drivaerFastback_2x120_xxxxx/log.* 

The job should run for about 8 minutes. 

## Visualize the results
Once the job completes, request an allocation for an HTC node so that you can utilize it to visualize results: 
```bash
salloc --partition=htc --nodes=1 --time=01:00:00 
```

From the deployer VM, use the Thinlinc client to connect to the HTC instance that was allocated.

Once you've connected to the HTC instance via the Thinlinc client, run the following command to launch Paraview: 
```bash
source /cvmfs/software.eessi.io/versions/2023.06/init/bash 

ml ParaView

paraview
```

From Paraview, open the case.foam file located in the last result directory ~/openfoam_tutorial_runs/drivaerFastback_2x120_xxxxx.

Then in the properties tab, select to display the following mesh regions:

- patch/body
- patch/frontWheels
- patch/rearWheels

Click on the +Y button and zoom to see a car. 

[Next Step: Run an AI Job ](./nemo.md)



