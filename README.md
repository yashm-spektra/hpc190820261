# ccws-learn

## Setup Pre-requisites
We will start the deployment by connecting to the Azure Cloud Shell. From the cloud Shell, confirm that your Azure CLI session has been configured to use the target subscription:

```
az account show
```
If the subscription listed is not the correct target for the deployment, update the selected subscription via the following command: 
```
az account set -s [subscription-id]
```
Use the following command to view the roles assigned to you at the tenant and subscription level: 
```
SUBSCRIPTION=$(az account show --query id --output tsv)
TENANT=$(az account show --query tenantId --output tsv)
UPN=$(az ad signed-in-user show --query "{UPN:userPrincipalName}" --output table --output tsv)

az role assignment list --all --assignee $UPN --output table | grep -E "${SUBSCRIPTION}$|${TENANT}$"
```
Verify that you have been assigned the following roles: 
- Contributor
- User Access Administrator


[Next Step: Setup Deployer](docs/setup.md)




