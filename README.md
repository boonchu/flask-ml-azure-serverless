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
   * [Create webapp](https://docs.microsoft.com/en-us/azure/developer/javascript/tutorial-vscode-azure-cli-node-03)
   * [Find instance costs per hour](https://azure.microsoft.com/en-us/pricing/details/app-service/linux/)
   * [ref: az webapp create](https://docs.microsoft.com/en-us/cli/azure/webapp?view=azure-cli-latest#az_webapp_create)

```
python3 -m venv ~/.flask-ml-azure
source ~/.flask-ml-azure/bin/activate
make install

# set default resource group
az group create --name linux-vm-rg 
az configure --defaults group=linux-vm-rg location=southeastasia

# Create 'F1' App service plan free tier
az appservice plan create --name myPlan --sku F1 -g linux-vm-rg --tags role=webapp --is-linux
az appservice plan list --query "[?sku.tier=='Free']" -o table

# find linux "runtime" type
az webapp list-runtimes --linux

# Create web app 'flask Machine Learning'
az webapp create --name flaskMLServerless --plan myPlan --runtime "PYTHON|3.8" -g linux-vm-rg --tags "role=webapp"
# list webapps.
az webapp list -o table                                                                 ──(Mon,Dec07)─┘
Name               Location        State    ResourceGroup    DefaultHostName                      AppServicePlan
-----------------  --------------  -------  ---------------  -----------------------------------  ----------------
flaskMLServerless  Southeast Asia  Running  linux-vm-rg      flaskmlserverless.azurewebsites.net  myPlan
# Bring application to browser
az webapp browse --name flaskMLServerless
```

   
