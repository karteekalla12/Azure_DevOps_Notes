# Azure CLI – Key Vault Management

## Create a Key Vault

```bash
az keyvault create \
  --name <KEY_VAULT_NAME> \
  --resource-group <RESOURCE_GROUP_NAME> \
  --location <LOCATION> \
  --sku <SKU> \
  --enabled-for-deployment <true|false> \
  --enabled-for-disk-encryption <true|false> \
  --enabled-for-template-deployment <true|false> \
  --tags key1=value1 key2=value2 \
  --no-wait \
  --verbose
````

---

## Parameter Explanations

| Parameter                           | Description                                                         |
| ----------------------------------- | ------------------------------------------------------------------- |
| `--name`                            | Name of the Key Vault (must be globally unique)                     |
| `--resource-group`                  | Resource group name                                                 |
| `--location`                        | Azure region                                                        |
| `--sku`                             | SKU type of the Key Vault                                           |
|                                     | Options: `standard`, `premium`                                      |
| `--enabled-for-deployment`          | Allow VM and other services to retrieve certificates from the vault |
|                                     | Values: `true` or `false`                                           |
| `--enabled-for-disk-encryption`     | Allow Azure Disk Encryption to use the vault                        |
|                                     | Values: `true` or `false`                                           |
| `--enabled-for-template-deployment` | Allow Azure Resource Manager templates to use the vault             |
|                                     | Values: `true` or `false`                                           |
| `--tags`                            | Key-value tags for resource organization                            |
| `--no-wait`                         | Do not wait for the operation to finish                             |
| `--verbose`                         | Enable verbose logging                                              |

---

## List Key Vaults in a Resource Group

```bash
az keyvault list --resource-group <RESOURCE_GROUP_NAME> --output table
```

---

## Show Details of a Key Vault

```bash
az keyvault show --name <KEY_VAULT_NAME> --resource-group <RESOURCE_GROUP_NAME> --output json
```

---

## Delete a Key Vault

```bash
az keyvault delete --name <KEY_VAULT_NAME> --resource-group <RESOURCE_GROUP_NAME>
```
