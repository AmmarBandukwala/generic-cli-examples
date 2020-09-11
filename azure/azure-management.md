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
