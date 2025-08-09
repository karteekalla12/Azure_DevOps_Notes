# Azure CLI – Storage Account Management

## Create a Storage Account

```bash
az storage account create \
  --name <STORAGE_ACCOUNT_NAME> \
  --resource-group <RESOURCE_GROUP_NAME> \
  --location <LOCATION> \
  --sku <SKU> \
  --kind <KIND> \
  --access-tier <ACCESS_TIER> \
  --enable-hierarchical-namespace <true|false> \
  --tags key1=value1 key2=value2 \
  --https-only <true|false> \
  --allow-shared-key-access <true|false> \
  --min-tls-version <TLS_VERSION> \
  --no-wait \
  --verbose
````

---

## Parameter Explanations

| Parameter                         | Description                                                                                            |
| --------------------------------- | ------------------------------------------------------------------------------------------------------ |
| `--name`                          | Globally unique name for the storage account (3-24 lowercase letters/numbers)                          |
| `--resource-group`                | Resource group name                                                                                    |
| `--location`                      | Azure region                                                                                           |
| `--sku`                           | Performance tier and replication strategy                                                              |
|                                   | Examples: `Standard_LRS`, `Standard_GRS`, `Standard_RAGRS`, `Premium_LRS`                              |
| `--kind`                          | Type of storage account                                                                                |
|                                   | Options: `StorageV2` (general purpose v2), `Storage`, `BlobStorage`, `FileStorage`, `BlockBlobStorage` |
| `--access-tier`                   | Access tier for BlobStorage accounts (Hot or Cool)                                                     |
|                                   | Values: `Hot`, `Cool`                                                                                  |
| `--enable-hierarchical-namespace` | Enables Data Lake Storage Gen2 features (true or false)                                                |
| `--tags`                          | Key-value tags for resource organization                                                               |
| `--https-only`                    | Allow only HTTPS traffic (true or false)                                                               |
| `--allow-shared-key-access`       | Allow shared key access for REST API (true or false)                                                   |
| `--min-tls-version`               | Minimum TLS version allowed (`TLS1_0`, `TLS1_1`, `TLS1_2`)                                             |
| `--no-wait`                       | Do not wait for the operation to finish                                                                |
| `--verbose`                       | Enable verbose logging                                                                                 |

---

## List Storage Accounts in a Resource Group

```bash
az storage account list --resource-group <RESOURCE_GROUP_NAME> --output table
```

---

## Show Details of a Storage Account

```bash
az storage account show --resource-group <RESOURCE_GROUP_NAME> --name <STORAGE_ACCOUNT_NAME> --output json
```

---

## Delete a Storage Account

```bash
az storage account delete --resource-group <RESOURCE_GROUP_NAME> --name <STORAGE_ACCOUNT_NAME> --yes
```

```

If you want similar Markdown for Blob Containers or File Shares, just ask!
```
