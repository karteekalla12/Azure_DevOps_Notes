# Azure CLI – Resource Group Management

## Login to Azure
```bash
az login
````

## Set Subscription (Optional)

```bash
az account set --subscription "<SUBSCRIPTION_ID_OR_NAME>"
```

## Create a Resource Group

```bash
az group create --name <RESOURCE_GROUP_NAME> --location <LOCATION>
```

**Example:**

```bash
az group create --name MyResourceGroup --location eastus
```

## List All Resource Groups

```bash
az group list --output table
```

## Show Resource Group Details

```bash
az group show --name <RESOURCE_GROUP_NAME> --output json
```

## Delete a Resource Group

```bash
az group delete --name <RESOURCE_GROUP_NAME> --yes --no-wait
```

```

Do you want me to also make a **cheatsheet like this** for **Azure Storage Account** so you can keep them in the same study format?
```
