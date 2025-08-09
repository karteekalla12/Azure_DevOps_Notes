# Azure CLI – Resource Group Management

```bash
# Login to Azure
az login

# Set subscription (optional)
az account set --subscription "<SUBSCRIPTION_ID_OR_NAME>"

# Create a resource group
az group create \
  --name <RESOURCE_GROUP_NAME> \
  --location <LOCATION> \
  [--tags key1=value1 key2=value2] \
  [--subscription <SUBSCRIPTION_ID>] \
  [--output table|json|yaml]

# List all resource groups
az group list \
  [--query <JMESPathQuery>] \
  [--output table|json|yaml]

# Show details of a specific resource group
az group show \
  --name <RESOURCE_GROUP_NAME> \
  [--query <JMESPathQuery>] \
  [--output table|json|yaml]

# Delete a resource group
az group delete \
  --name <RESOURCE_GROUP_NAME> \
  --yes \
  [--no-wait] \
  [--subscription <SUBSCRIPTION_ID>]
