Microsoft Azure Cloud Command Line - Snippets

## Create Virtual Network for VM

```shell
az network vnet create --resource-group myresourcegroup --name vnet-my-test --address-prefix 192.168.0.0/16 --subnet-name vnet-my-test-subnet --subnet-prefix 192.168.1.0/24
```

## Create Network Security Group for VM

```shell
az network nsg create --resource-group myresourcegroup --name nsg-my-test
```

## Create IP and Network Adapter
```shell
az network public-ip create --resource-group myresourcegroup --name publicip-rabbitmq-my-test --dns-name testrabbitmq
az network nic create --resource-group myresourcegroup --name nic-rabbitmq-my-test --vnet-name vnet-my-test --subnet vnet-my-test-subnet --public-ip-address publicip-rabbitmq-my-test --network-security-group nsg-my-test
```
## Create Azure VM

```shell
az vm create --resource-group myresourcegroup --name centos-rabbitmq-test --image OpenLogic:CentOS:7.3:latest --nics nic-rabbitmq-my-test --admin-username abandukwala --generate-ssh-keys
```
