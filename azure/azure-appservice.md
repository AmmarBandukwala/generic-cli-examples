# Microsoft Azure Cloud Command Line - Snippets

## Get Publish Profiles UN/PW for an App Service

_TIP: Login via KUDU using SCM URL of App Service - <https://{appservicename}.azurewebsites.net/basicAuth>_

```shell
az webapp deployment list-publishing-profiles --name {appservicename} --resource-group {resourcegroupname} --subscription {subguid}
```

## Set an app service environment variable i.e web.config/appsettings.json

```shell
az webapp config appsettings set -g {resourcegroupname} -n {appservicename} --settings {key}={value}
```

## Update EasyAuth Azure AD app service configuration

```shell
az webapp auth update --aad-client-id {clientid} --aad-client-secret {clientsecret} --name {appservicename} --resource-group {resourcegroupname} --subscription {subguid}
```

## Import SSL Certificate from KeyVault into app service

```shell
az webapp config ssl import --resource-group {resourcegroupname} --name {appservicename} --key-vault {keyvaultname} --key-vault-certificate-name {certname}
```
