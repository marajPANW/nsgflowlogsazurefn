This project installs into an Azure Function in your Azure subscription. Its job is to read NSG Flow Logs from your configured storage account, break the data into chunks that are the right size for your log analytics system to ingest, then transmit the chunks to that system. At present, you may choose from two output bindings: XDR, Event Hub.  


[![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FmarajPANW%2Fnsgflowlogsazurefn%2Fmain%2FNwNsgProject%2Fdeploy.json)


NOTE regarding the Event Hub output binding:  

Native support for event hubs is not yet available, but would be the preferred method. If you use prefer to send NSG flow logs to XDR using event hub rather than XDR HTTP Collector, the event hub output binding will do the job. In [Azure Monitor Addon For Splunk](https://github.com/Microsoft/AzureMonitorAddonForSplunk), configure the Azure Monitor Diagnostic Logs data input and add a line to ```TA-folder/bin/app/hubs.json``` similar to this:  

Example: ```'insights-logs-nsgflowlogs': 'resourceId'```  

When you create the hub (e.g. ```insights-logs-nsgflowlogs```) set the number of partitions to 4. This is mandatory.

# Settings

In the Application Settings of your Azure Function:
* AppName                     - this is the name of the function app. In the Azure Portal, this is the name that will appear in the list of resources.  
   Example: ```MyNSGApp```  
* appServicePlan              - "ServicePlan" or "Consumption".  
   If you select "ServicePlan", an App Service Plan will be created and you will be billed accordingly. If you select "Consumption", you will be billed based on the Consumption plan.  
* appServicePlanTier          - "Free", "Shared", "Basic", "Standard", "Premium", "PremiumV2"  
   Example: ```Standard```  
   (only relevant for ServicePlan)  
* appServicePlanName          - depends on tier, for full details see "Choose your pricing tier" in the portal on an App service plan "Scale up" applet.  
   Example: For standard tier, "S1", "S2", "S3" are options for plan name  
   (only relevant for ServicePlan)  
* appServicePlanCapacity      - how many instances do you want to set for the upper limit?  
   Example: For standard tier, S2, set a value from 1 to 10  
   (only relevant for ServicePlan)  
* githubRepoURL                     - this is the URL of the repo that contains the function app source. You would put your fork's address here.  
   Example: ```https://github.com/PaloAltoNetworks/AzureNetworkWatcherNSGFlowLogsConnector```
* githubRepoBranch                  - this is the name of the branch containing the code you want to deploy.  
   Example: ```master```  
* nsgSourceDataConnection     - a storage account connection string  
   Example: ```DefaultEndpointsProtocol=https;AccountName=yyy;AccountKey=xxx;EndpointSuffix=core.windows.net```
* xdrHost - XDR HTTP collector endpoint
* xdrToken - XDR HTTP collector token
