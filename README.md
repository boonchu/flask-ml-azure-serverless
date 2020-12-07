#### flask-ml-azure-serverless

   * Project Flask Machine Learning with Azure Serverless

#### How to integrate with 'Azure Devops/Pipelines' services

   * [Azure DevOps guide for setup](https://www.azuredevopslabs.com/labs/azuredevops/github-integration/)
   * Use marketplace in github to link with Azure DevOps account.
   * [at default landing page Azure Devops shoud have 'github project'](https://dev.azure.com/)
   
   
#### How to deploy webapp

   * checkout this code.
   * install in azure cloud shell.

```
python3 -m venv ~/.flask-ml-azure
source ~/.flask-ml-azure/bin/activate
make install
az appservice plan create --name myPlan --sku F1 -g vm-rg --tags role=webapp
az webapp up --name flaskMLServerless --resource-group vm-rg --plan myPlan
```
