## Create Deployer VM

From the Azure cloud shell, run the following command to create a resource group: 

```bash
RG=[name of resource group]
REGION=[preferred region]
az group create --name $RG --location $REGION

```

We will use a deployer VM and connect to it via Bastion to deploy the cluster environment. To create the deployer VM, run the following command. Note, update the size of the deployer VM based on the available capacity in your target region:

```bash
ADMIN=hpcadmin
PASSWORD=[your password]
SIZE=Standard_D2s_v5

az vm create --resource-group $RG --name deployerWinVM --image Win2022Datacenter --admin-username $ADMIN --admin-password $PASSWORD --size $SIZE --public-ip-address ""
```

## Install Required Software

The VM is deployed with no public-ip address, so we will need to log in to it via Bastion. From the Azure Portal, click on your VM resource and then go to **Settings** > **Bastion**. From there, select the VM you deployed, enter your credentials, and click **Connect**. 

Once you have connected to the deployer VM, you will need to install the [Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-windows?view=azure-cli-latest&pivots=msi), [Git](https://git-scm.com/install/windows), and [winget](https://learn.microsoft.com/en-us/windows/package-manager/winget/). Use winget to install [jq](https://jqlang.org/download/). 

After installing the required software, open Git Bash and run: 

```bash
az login
```
Log into your account and select the subscription that you will target for the deployment. 

The deployment will require you to provide ssh-keys. In order to generate them, from the git bash terminal, run the following command: 

```bash
ssh-keygen -t ed25519 -C "hpcadmin"
```
When prompted, do not enter a passphrase to create the key.

Finally, clone this repo, to begin your deployment. 
```bash
git clone [url to clone this repo]
```

[Next: Deploy Environment](./deploy.md)



