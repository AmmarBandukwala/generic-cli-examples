## Script to List Cost via Powershell

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
