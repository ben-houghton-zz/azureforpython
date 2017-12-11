# Get Started with Azure Functions

## Exercise 1 - Create a new Azure Function App
For a Function to operate, it needs some underlying infrastructure. There are 2 supported types
of resources supported for Azure Functions

1. **AppService Plan** (predictable workloads, prepaid cost)
2. **Consumption Plan** (unpredictable workloads, pay-as-you-go)

For a FunctionApp to operate, we first need to create an Azure Storage Account which is use to store
important information such as logs etc and it's required by default by both plans.

### Create a Storage Account
Storage accounts are a resource like all other resources on Azure. You can use the CLI to create
a new storage account. Optionally, you can use the `--no-wait` option to speed things up
    
`az storage account create --name <yourInitials>demostorage<Random3DigitNumber> --kind Storage --resource-group <The RG Name> --sku Standard_LRS  --no-wait`

### Create an AppService Plan FunctionApp
We can easily create and deploy some code to our function app using the following command:

`az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--consumption-plan-location southuk --deployment-source-url https://github.com/Azure-Samples/functions-quickstart`

Setting the `--consumption-plan-location` value means that the FunctionApp will run on the Consumption Plan. Checking the resource template for this, you'll notice that this assigns a `--sku Dynamic` to the 
FunctionApp 

> HINT: you can't set the Dynamic SKU manually

The `--deployment-source-url` deploys the sample code. This could be set to any public repository that contains Azure Functions code.

### Test the function 

The easiest way to test an HttpTrigger Function is to use `curl`. You'll need to know both the name of the FunctionApp and the name of the Function we need to call in order for Curl to work. The final command should look like this:

`curl http://<app_name>.azurewebsites.net/api/<FunctionName>?name=<yourname>`

### Update the Python version for App Service

https://github.com/MicrosoftDocs/visualstudio-docs/blob/master/docs/python/managing-python-on-azure-app-service.md
