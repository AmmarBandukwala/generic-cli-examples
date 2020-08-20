# Microsoft Azure Cloud Command Line - Snippets

## Login using AZ CLI from Microsoft

```shell
az login --subscription {subguid} --service-principal --username {username} --password {password} --tenant {tenantguid}
az account set --subscription {subguid}
```

## Login using PowerShell Modules

```bash
$passwd = ConvertTo-SecureString "{clientsecret}" -AsPlainText -Force
$pscredential = New-Object System.Management.Automation.PSCredential('{clientid}', $passwd)
Connect-AzAccount -ServicePrincipal -Credential $pscredential -Tenant '{tenantguid}'
```
