
# Windows Virtual Desktop Scaling Tool (ANS Custom)

## How To

1. Open Windows PowerShell
2. Run the following cmdlet to sign in to your Azure account. ( Note: Your account must have contributor rights on the Azure subscription where you want to deploy the scaling tool. )
```powershell
Login-AzAccount
```

3. Run the following cmdlet to download the script for creating the Azure Automation account:
```powershell
New-Item -ItemType Directory -Path "C:\Temp" -Force
Set-Location -Path "C:\Temp"
$Uri = "https://raw.githubusercontent.com/ans-cloud/RDS-Templates/master/wvd-templates/wvd-scaling-script/CreateOrUpdateAzAutoAccount.ps1"
# Download the script
Invoke-WebRequest -Uri $Uri -OutFile ".\CreateOrUpdateAzAutoAccount.ps1"
```

4. Run the following cmdlet to execute the script and create the Azure Automation account. You can either fill in values for the parameters or comment them to use their defaults.
```powershell
$Params = @{
     "AADTenantId"           = "<Azure_Active_Directory_tenant_ID>"   # Optional. If not specified, it will use the current Azure context
     "SubscriptionId"        = "<Azure_subscription_ID>"              # Optional. If not specified, it will use the current Azure context
     "UseARMAPI"             = $true                                  # (This must be set to $true to use the ANS custom version)
     "ResourceGroupName"     = "<Resource_group_name>"                # Optional. Default: "WVDAutoScaleResourceGroup"
     "AutomationAccountName" = "<Automation_account_name>"            # Optional. Default: "WVDAutoScaleAutomationAccount"
     "Location"              = "<Azure_region_for_deployment>"
     "WorkspaceName"         = "<Log_analytics_workspace_name>"       # Optional. If specified, Log Analytics will be used to configure the custom log table that the runbook PowerShell script can send logs to
}

.\CreateOrUpdateAzAutoAccount.ps1 @Params
```

5. The cmdlet's output will include a webhook URI. Make sure to keep a record of the URI because you'll use it as a parameter when you set up the execution schedule for the Azure Logic App.

6. If you specified the parameter WorkspaceName for Log Analytics, the cmdlet's output will also include the Log Analytics Workspace ID and its Primary Key. Make sure to remember URI because you'll need to use it again later as a parameter when you set up the execution schedule for the Azure Logic App.

7. After you've set up your Azure Automation account, sign in to your Azure subscription and check to make sure your Azure Automation account and the relevant runbook have appeared in your specified resource group.

8. To check if your webhook is where it should be, select the name of your runbook. Next, go to your runbook's Resources section and select Webhooks.

## For the ARM based WVD using the new 2020 Spring update (Microsoft)

Review this [documentation](https://docs.microsoft.com/en-us/azure/virtual-desktop/set-up-scaling-script) for deployment guidance.

