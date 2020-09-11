# Microsoft Azure Cloud Command Line - Snippets

## List and Output Azure Consumption

```powershell
$accounts = az account list | ConvertFrom-Json
foreach ($account in $accounts) {

    az account set -s $account.id
    
    Write-Host "Start of Subscription " $account.id
    
    $items = az consumption budget list --query "[].{Id:id, Amount:amount, CurrentAmount:currentSpend.amount, Name:name}" | ConvertFrom-Json

    foreach ($item in $items){
        $output = [pscustomobject]@{            
            "Subscription" = $account.name
            "Id" = $item.id
            "Name" = $item.name
            "Amount" = $item.amount
            "CurrentAmount" = $item.currentAmount
            "Progress" = ([convert]::ToDouble($item.currentAmount) / [convert]::ToDouble($item.amount)) * 100.00
        }
        
        $output | Export-Csv -Path ".\budgetreport.csv" -NoTypeInformation -Append -Force    
    }

    Write-Host "End of Subscription " $account.id $set  
}
```

## List and Output Advisor Recommendations

```powershell
$accounts = az account list | ConvertFrom-Json
foreach ($account in $accounts) {
    Write-Host "Start" $account.id
    $objects = az advisor recommendation list --category Cost --subscription $account.id | ConvertFrom-Json
    foreach ($obj in $objects) {
        $output = [pscustomobject]@{
            "Subscription" = $account.name
            "Resource Group" = $obj.resourceGroup
            "Impact" = $obj.impact
            "ImpactedField" = $obj.impactedField
            "ImpactedValue" = $obj.impactedValue
            "Problem" = $obj.shortDescription.problem
        }
        $output | Export-Csv -Path ".\report.csv" -NoTypeInformation -Append         
    }
    Write-Host "End" $account.id
}
```

## Example Call RESTApi to Deploy a Budget via ARM Template

```powershell
$subscriptionId = $account.id
$groupName = $group.Name
$budgetName = $group.Name
$email = $group.OwnerEmail

$armTemplate = (Get-Content "arm.json" -Raw) | ConvertFrom-Json
$armTemplate.properties.notifications.Actual_GreaterThan_80_Percent.contactEmails += $email

$jsonBody = $armTemplate | ConvertTo-Json -Depth 4

# REST Way
# PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Consumption/budgets/{budgetName}?api-version=2019-01-01                       
$Token = az account get-access-token --subscription $account.id | ConvertFrom-Json
$apiUri = "https://management.azure.com/subscriptions/"+$subscriptionId+"/resourceGroups/"+$groupName+"/providers/Microsoft.Consumption/budgets/"+$budgetName+"?api-version=2019-05-01"                 
$Headers = @{}
$Headers.Add("Authorization","$($Token.tokenType) "+ " " + "$($Token.accessToken)")
$newBudget = Invoke-RestMethod -ContentType 'application/json' -Method DELETE -Uri $apiUri -Headers $Headers
$newBudget = Invoke-RestMethod -ContentType 'application/json' -Method PUT -Uri $apiUri -Headers $Headers -Body $jsonBody

# $newBudget = az consumption budget create `
#               --budget-name $budgetName `
#               --category cost --amount 500.00 `
#               --end-date 2020-12-31T00:00:00Z `
#               --start-date 2019-07-01T00:00:00Z `
#               --time-grain monthly `
#               --resource-group $groupName `
#               --subscription $subscriptionId
```
