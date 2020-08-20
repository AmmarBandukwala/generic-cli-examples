# Microsoft Azure Cloud Command Line - Snippets

## Retrieve Azure Storage Account connection string

```shell
az storage account show-connection-string -g {resourcegroupname} -n {storageaccountname}
```

## Generate SAS Token from Azure Storage Blob Container

```shell
az storage container generate-sas --account-key {connectionkey1} --account-name {storageaccountname} --expiry 2021-12-01 --name {containername} --permissions acdlpruw
```

## Az Copy (Seperate Utility) source storage account to target

```shell
azcopy copy "{sourceSASUrl}" "{targetSASUrl}"
```
