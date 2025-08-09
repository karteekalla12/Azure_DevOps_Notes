# Azure CLI – Virtual Machine Management

## Create a Virtual Machine

```bash
az vm create \
  --resource-group <RESOURCE_GROUP_NAME> \
  --name <VM_NAME> \
  --image <IMAGE> \
  --size <VM_SIZE> \
  --admin-username <ADMIN_USERNAME> \
  --admin-password <ADMIN_PASSWORD> \
  --authentication-type <AUTH_TYPE> \
  --ssh-key-values <SSH_PUBLIC_KEY_PATH> \
  --location <LOCATION> \
  --storage-sku <STORAGE_SKU> \
  --os-disk-name <OS_DISK_NAME> \
  --os-disk-size-gb <OS_DISK_SIZE> \
  --public-ip-address <PUBLIC_IP_NAME> \
  --public-ip-sku <PUBLIC_IP_SKU> \
  --vnet-name <VNET_NAME> \
  --subnet <SUBNET_NAME> \
  --nsg <NSG_NAME> \
  --tags key1=value1 key2=value2 \
  --no-wait \
  --verbose
````

---

## Parameter Explanations

| Parameter               | Description                                                                                          |
| ----------------------- | ---------------------------------------------------------------------------------------------------- |
| `--resource-group`      | Name of the resource group where the VM will be created                                              |
| `--name`                | Name of the virtual machine                                                                          |
| `--image`               | OS image to use. Can be shorthand (e.g., `UbuntuLTS`) or full reference (publisher\:offer\:sku\:ver) |
| `--size`                | VM size specifying CPU, memory, and other resources                                                  |
| `--admin-username`      | Admin username for logging into the VM                                                               |
| `--admin-password`      | Password for admin user (required if using password authentication)                                  |
| `--authentication-type` | Authentication method: `ssh` or `password`                                                           |
| `--ssh-key-values`      | SSH public key path for SSH authentication                                                           |
| `--location`            | Azure region                                                                                         |
| `--storage-sku`         | Storage type for OS disk, e.g., `Standard_LRS`, `Premium_LRS`                                        |
| `--os-disk-name`        | Name of the OS disk                                                                                  |
| `--os-disk-size-gb`     | Size of the OS disk in GB                                                                            |
| `--public-ip-address`   | Name of the public IP resource                                                                       |
| `--public-ip-sku`       | SKU of the public IP address: `Basic` or `Standard`                                                  |
| `--vnet-name`           | Name of the Virtual Network                                                                          |
| `--subnet`              | Subnet name inside the Virtual Network                                                               |
| `--nsg`                 | Network Security Group to associate with the VM                                                      |
| `--tags`                | Key-value tags for resource organization                                                             |
| `--no-wait`             | Do not wait for the operation to complete                                                            |
| `--verbose`             | Enable verbose logging                                                                               |

---

## Other Useful VM Commands

Start a VM:

```bash
az vm start --resource-group <RESOURCE_GROUP_NAME> --name <VM_NAME>
```

Stop (deallocate) a VM:

```bash
az vm stop --resource-group <RESOURCE_GROUP_NAME> --name <VM_NAME>
```

Restart a VM:

```bash
az vm restart --resource-group <RESOURCE_GROUP_NAME> --name <VM_NAME>
```

Delete a VM:

```bash
az vm delete --resource-group <RESOURCE_GROUP_NAME> --name <VM_NAME> --yes
```
