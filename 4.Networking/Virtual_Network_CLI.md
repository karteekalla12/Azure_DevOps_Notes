# Azure CLI – Virtual Network (VNet) Management

## Create a Virtual Network

```bash
az network vnet create \
  --resource-group <RESOURCE_GROUP_NAME> \
  --name <VNET_NAME> \
  --address-prefix <ADDRESS_PREFIX> \
  --subnet-name <SUBNET_NAME> \
  --subnet-prefix <SUBNET_PREFIX> \
  --location <LOCATION> \
  --tags key1=value1 key2=value2 \
  --no-wait \
  --verbose
````

---

## Parameter Explanations

| Parameter          | Description                                     |
| ------------------ | ----------------------------------------------- |
| `--resource-group` | Name of the resource group to contain the VNet  |
| `--name`           | Name of the Virtual Network                     |
| `--address-prefix` | CIDR block for the VNet (e.g., `10.0.0.0/16`)   |
| `--subnet-name`    | Name of the initial subnet within the VNet      |
| `--subnet-prefix`  | CIDR block for the subnet (e.g., `10.0.0.0/24`) |
| `--location`       | Azure region                                    |
| `--tags`           | Key-value pairs to tag the resource             |
| `--no-wait`        | Do not wait for the operation to finish         |
| `--verbose`        | Enable verbose logging                          |

---

## Add a Subnet to an Existing VNet

```bash
az network vnet subnet create \
  --resource-group <RESOURCE_GROUP_NAME> \
  --vnet-name <VNET_NAME> \
  --name <SUBNET_NAME> \
  --address-prefix <SUBNET_PREFIX> \
  --network-security-group <NSG_NAME> \
  --route-table <ROUTE_TABLE_NAME> \
  --service-endpoints <SERVICE1 SERVICE2> \
  --delegations <DELEGATION_NAME> \
  --no-wait \
  --verbose
```

### Parameters for Subnet Creation

| Parameter                  | Description                                                  |
| -------------------------- | ------------------------------------------------------------ |
| `--resource-group`         | Resource group name                                          |
| `--vnet-name`              | Name of the virtual network to add the subnet to             |
| `--name`                   | Name of the subnet                                           |
| `--address-prefix`         | CIDR block for the subnet (e.g., `10.0.1.0/24`)              |
| `--network-security-group` | Associate an NSG with the subnet (optional)                  |
| `--route-table`            | Associate a route table with the subnet (optional)           |
| `--service-endpoints`      | Enable service endpoints like `Microsoft.Storage` (optional) |
| `--delegations`            | Delegate subnet to specific Azure service (optional)         |
| `--no-wait`                | Do not wait for operation to finish (optional)               |
| `--verbose`                | Enable verbose logging                                       |

---

## List Virtual Networks in a Resource Group

```bash
az network vnet list --resource-group <RESOURCE_GROUP_NAME> --output table
```

---

## Show Details of a Virtual Network

```bash
az network vnet show --resource-group <RESOURCE_GROUP_NAME> --name <VNET_NAME> --output json
```

---

## Delete a Virtual Network

```bash
az network vnet delete --resource-group <RESOURCE_GROUP_NAME> --name <VNET_NAME> --yes
```

```

If you want, I can provide similar Markdown for Network Security Groups or Route Tables!
```
