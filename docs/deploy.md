## Deploy HPC+AI Cluster

Use notepad to open `env.txt`. You can locate the file under the following path: 
```
C:\Users\hpcadmin\ccws-learn
```

Update the values from `env.txt` that are surrounded by square brackets: 
```bash
SUB=[Your subscription]
export RG=[Your resource group]
export REGION=[Your preferred region]
ADMIN=hpcadmin
PASSWORD=[password for admin account]
HTC=Standard_FX2ms_v2
HTC_COUNT=10
HPC=Standard_HB120rs_v2
HPC_COUNT=4
GPU=Standard_NC96ads_A100_v4
GPU_COUNT=1
SCHED=Standard_D8as_v5
LOGIN=Standard_D4as_v5
ZONE=1
VNET_ADDR="10.200.0.0/16"
ANF_SKU=Premium
ANF_SIZE=4
AMLFS_SKU=AMLFS-Durable-Premium-500
AMLFS_SIZE=8
SACCTDB=[name for accounting db]
OOD=Standard_D8as_v5
DOMAIN=[Your domain]
export MI_NAME=[Desired name of the managed identity resource]
export APP_NAME=[Desired name of the Microsoft Entra ID application registration]
COMMIT=e818de6
CID=""
MID=""
TID=""

```

Once defined, run the following commands to load the values into the environment of your shell session:

```
source env.txt
```

Execute the following script to create a Microsoft Entra application registration for use with Azure CycleCloud:

```
./scripts/entra_predeploy.sh
```
The script will output the values of your Tenant ID, Client ID, and Managed Identity Resource ID for the application you created. 

Open `env.txt` again and update the values of CID, MID, and TID, with the following values: 

```
CID=[value of Client ID]
MID=[value of Managed Identity Resource ID]
TID=[value of Tenant ID]
```

Source `env.txt` again so that the updated values are loaded into the environment of your shell session: 

```
source env.txt
```

Run the `deploy-ccws.sh` script to deploy the HPC cluster:  

```bash
 ./scripts/deploy-ccws.sh --subscription-id $SUB --resource-group $RG --location $REGION --ssh-public-key-file ~/.ssh/id_ed25519.pub --admin-password $PASSWORD --admin-username $ADMIN --htc-sku $HTC --hpc-sku $HPC --gpu-sku $GPU --scheduler-sku $SCHED --login-sku $LOGIN --htc-az $ZONE --hpc-az $ZONE --gpu-az $ZONE --htc-max-nodes $HTC_COUNT --hpc-max-nodes $HPC_COUNT --gpu-max-nodes $GPU_COUNT --network-address-space $VNET_ADDR --anf-sku $ANF_SKU --anf-size $ANF_SIZE --anf-az $ZONE --amlfs-sku $AMLFS_SKU --amlfs-size $AMLFS_SIZE --amlfs-az $ZONE --create-accounting-mysql --db-name $SACCTDB --db-user $ADMIN --db-password $PASSWORD --open-ondemand --ood-sku $OOD  --ood-user-domain $DOMAIN --entra-id --entra-app-umi $MID --entra-app-id $CID --workspace-ref main --output-file my-deployment-params.json --accept-marketplace --specify-az --workspace-commit $COMMIT --deploy
```

Once the deployment completes, run the following command to update your Entra App's redirect URI's to point to your deployment's resources: 

```
LATEST_RELEASE=$(curl -sSL -H 'Accept: application/vnd.github+json' "https://api.github.com/repos/Azure/cyclecloud-slurm-workspace/releases/latest" | sed -n 's/.*"tag_name":[[:space:]]*"\([^"]*\)".*/\1/p')

bash <(curl -sL "https://raw.githubusercontent.com/Azure/cyclecloud-slurm-workspace/refs/tags/${LATEST_RELEASE}/util/entra_postdeploy.sh") -rg $RG
```


Finally, you will find that the deployment created a separate vnet for the cluster called `ccws-vnet`. Use the Azure portal to create a peering between the deployer's VNET and the cluster's VNET. Once the peering is established, you will be able to connect to the cluster. 

[Next Step: Connect ](./connect.md)


