#### flask-ml-azure-serverless

   * Project Flask Machine Learning with Azure Serverless

#### How to integrate with 'Azure Devops/Pipelines' services

   * [Azure DevOps guide for setup](https://www.azuredevopslabs.com/labs/azuredevops/github-integration/)
   * Use marketplace in github to link with Azure DevOps account.
   * [at default landing page Azure Devops shoud have 'github project'](https://dev.azure.com/)
   
   
#### How to deploy webapp

   * [See the 'flask' example](https://docs.microsoft.com/en-us/azure/app-service/quickstart-python?tabs=bash&pivots=python-framework-flask)
   * checkout this code.
   * install in azure cloud shell.
   * [List service plan](https://docs.microsoft.com/en-us/cli/azure/appservice/plan?view=azure-cli-latest#az_appservice_plan_list)
   * [Build webapp](https://docs.microsoft.com/en-us/azure/devops/pipelines/ecosystems/python?view=azure-devops)
   * [Create webapp](https://docs.microsoft.com/en-us/azure/devops/pipelines/ecosystems/python-webapp?view=azure-devops)
   * [Find instance costs per hour](https://azure.microsoft.com/en-us/pricing/details/app-service/linux/)
   * [ref: az webapp create](https://docs.microsoft.com/en-us/cli/azure/webapp?view=azure-cli-latest#az_webapp_create)


#### Follows pipeline instructions

   * [bring your modern data pipeline to production](https://towardsdatascience.com/how-to-bring-your-modern-data-pipeline-to-production-2f14e42ac200)

     - Create service end point flask web app
     - Create Service Connection to fetch ID


```
# Create service end point flask web app
python3 -m venv ~/.flask-ml-azure
source ~/.flask-ml-azure/bin/activate
make install

# set default resource group
az group create --name linux-vm-rg --location southeastasia
az configure --defaults group=linux-vm-rg location=southeastasia

# Create 'F1' App service plan free tier
az appservice plan create --name myPlan --sku F1 -g linux-vm-rg --tags role=webapp --is-linux
az appservice plan list --query "[?sku.tier=='Free']" -o table

# find linux "runtime" type
az webapp list-runtimes --linux

# Create web app 'flask Machine Learning'
az webapp create --name flaskMLServerless --plan myPlan --runtime "PYTHON|3.6" -g linux-vm-rg --tags "role=webapp"

# See Notes for details.
az webapp config appsettings set -g linux-vm-rg -n flaskMLServerless --settings SCM_DO_BUILD_DURING_DEPLOYMENT=true

# List webapps.
az webapp list -o table
Name               Location        State    ResourceGroup    DefaultHostName                      AppServicePlan
-----------------  --------------  -------  ---------------  -----------------------------------  ----------------
flaskMLServerless  Southeast Asia  Running  linux-vm-rg      flaskmlserverless.azurewebsites.net  myPlan

# Bring application to browser
az webapp browse --name flaskMLServerless

# Delete webapp
az webapp delete --name flaskMLServerless
```

```
# Create Service Connection to fetch ID
- Create new project 'flask-ml-azure-serverless'
- Configure 'project settings' -> 'Settings' -> 'Service Connections' 
- You need 'WebApp Name', 'Subscription id', 'Resource Group'
- Configure 'Azure Resource Manager'
- Configure pipeline to allow to fetch github project
- Update Service Connection ID in pipeline YAML
```

#### Notes

```
Important Notes

If your app fails because of a missing dependency, then your requirements.txt file was not processed 
during deployment. This behavior happens if you created the web app directly on the portal rather than 
using the az webapp up command as shown in this article.

The 'az webapp up' command specifically sets the build action 

'SCM_DO_BUILD_DURING_DEPLOYMENT = true'. 

If you provisioned the app service through the portal, however, this action is not automatically set.

The following steps set the action:

- Open the Azure portal, select your App Service, then select Configuration.
- Under the Application Settings tab, select New Application Setting.
- In the popup that appears, set Name to SCM_DO_BUILD_DURING_DEPLOYMENT, set Value to true, and select OK.
- Select Save at the top of the Configuration page.
- Run the pipeline again. Your dependencies should be installed during deployment.
```
   
