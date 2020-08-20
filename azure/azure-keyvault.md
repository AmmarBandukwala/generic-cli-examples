# Microsoft Azure Cloud Command Line - Snippets

## Show KeyVault properties and settings

```shell
az keyvault show --name {appservicename} --resource-group {resourcegroupname}
```

## List all soft-deleted items in KeyVault

```shell
az keyvault secret list-deleted --vault-name {keyvaultname}
```

## Permantly delete a soft-deleted secret from the KeyVault

```shell
az keyvault secret purge --vault-name {keyvaultname} --name {secretname}
```

## Set an access policy for a user/group/service-principal to the KeyVault

```shell
az keyvault set-policy --name {keyvaultname} --secret-permissions backup, delete, get, list, purge, recover, restore, set --certificate-permissions backup, create, delete, get, getissuers, import, list, listissuers, recover, restore, update --resource-group {resourcegroupname} --subscription {subguid} --object-id {guid}
```

```shell
az keyvault delete-policy --name {keyvaultname} --resource-group {resourcegroupname} --subscription {subguid} --object-id {guid}
```
