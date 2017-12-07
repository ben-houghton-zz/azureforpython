
# Azure for Python Developers

 In this workshop, we are going to - 

			1. Create a Jupyter Notebook in notebooks.azure.com
			2. Share the notebook with users in the workshop
			3. Install Azure SDK to notebook
			4. Show how to create Azure resources in notebook using Azure SDK
			5. Show how to call a Function API as a REST service in Python & notebook
			6. Show how to use Key Vault to secure API Keys and use this in the Python script
			7. Deploy a Python script as a Chron initiated Azure WebJob
			8. Exercise: Create notebook. Add Azure SDK. Call predefined Python Echo function using supplied 
			   API key via the Notebook

 ### 1). Create a SaaS Jupyter Notebook
 
 Navigate your browser to the Microsoft Azure Notebooks Url https://notebooks.azure.com
 
 Sign in with your Azure or CORP credentials
  
 <img src="https://github.com/ben-houghton/azureforpython/blob/master/images/notebooksstart.PNG" width="700">
 
 

Create a library by clicking on the '+' icon 


<img src="https://github.com/ben-houghton/azureforpython/blob/master/images/notebookslogin.PNG" width="700">
 
 
  
Add a new Notebook to the library. If this is the one to share, once you have created it ensure that you go to the settings menu and  your check the 'public' checkbox.


 <img src="https://github.com/ben-houghton/azureforpython/blob/master/images/notebooknew.PNG" width="700">




### 2). Share the notebook with other users


Share the Notebook by clicking on the 'Share' icon and in the dialog box, add a list of users with whom you wish to share.


<img src="https://github.com/ben-houghton/azureforpython/blob/master/images/notebookshare.PNG" width="700">




### 3). Install the Azure Python SDK

Navigate to the notebook you created by double-clicking on it's name in the library

To install the Azure SDK package type -

```javascript
	!pip install azure
```

<img src="https://github.com/ben-houghton/azureforpython/blob/master/images/jnotebookinstallazure.PNG" width="700">



This will allow you to write scripts in Python that consume the Azure services and resources in your Azure Subscription


The best place to start with help for building Python Apps that run on or integrate with Azure is the Azure Python Developer Center https://azure.microsoft.com/en-us/develop/python/. You can find lots of examples and development scenarios there and also the Azure Python SDK reference http://azure-sdk-for-python.readthedocs.io/en/latest/


### 4). Create Azure Resources using the Azure Python SDK

There are some good examples of how to integrate Python and the Azure SDK. This example https://github.com/Azure-Samples/storage-blobs-python-quickstart demonstates how to use the Azure SDK to add files to a storage account. 

You'll first need to install the Azure Storage SDK

```javascript
	!pip install azure-storage-blob
```
The copy the code from this example https://github.com/Azure-Samples/storage-blobs-python-quickstart/blob/master/example.py to your https://notebooks.azure.com notebook.

If you follow the instructions to create a storage account through the Azure Portal and then run the script using you storage account name and keys you see the script execute.

You'll need to make one change to the script. It looks for a relative directory that doesn't exist, so simply just remove the reference to 'Documents'.

Before -
<img src="https://github.com/ben-houghton/azureforpython/blob/master/images/scripteditorig.PNG" width="700">


After -
<img src="https://github.com/ben-houghton/azureforpython/blob/master/images/scriptedit.PNG" width="700">


When the script runs in your notebook, it's create a new Blob Container and add a file.

<img src="https://github.com/ben-houghton/azureforpython/blob/master/images/scriptrun.PNG" width="700">

### 5). Call a Function API as a REST service in Python & notebook

Once I have started creating APIs in Azure using services like Functions or API Apps I can make these available to my scripts. The APIs will be able to perform some sort of heavy duty process that takes a request and returns a response. In this instance I have created a simple Python based Serverless HTTP Function that call be called as a REST service in my Python script. I've made the Function public and ensured that an API key needs to be provided to get access - 

https://benhdemo.azurewebsites.net/api/Pydemo?code=[apikey]

The API can be called in a Python script

```javascript
	import requests, json
	api_url = "https://benhdemo.azurewebsites.net/api/Pydemo?code=[apikey]"
	data = json.dumps({'name':'Ben H'})
	r = requests.post(api_url, data)
	print(r.text)
```

<img src="https://github.com/ben-houghton/azureforpython/blob/master/images/functioncall.PNG" width="700">

### 6). Use Key Vault to secure API Keys and use this in the Python script

In the previous section we embedded the API key within the URL which coudl be easily retrieved from within our source code. The answer to this insecurity is to hold our API Key in Azure Key Vault and to access it securely using REST call and a Service Principle identity.

There is a great overview for you to follow here - https://docs.microsoft.com/en-us/python/api/overview/azure/key-vault?view=azure-python 

The implementation of this may be spilt into a number of scripts but effectively you need to make a call to Key Vault using a secure Service Principle identity to obtain the API Key and then append the returned value to your API call.

Something like this - 

```javascript
	import requests, json
	from azure.keyvault import KeyVaultClient, KeyVaultAuthentication
	from azure.common.credentials import ServicePrincipalCredentials
	from azure.keyvault import KeyVaultId

	vault_url = '[key vault url]'
	secret_name = '[key vault secret name]'

	# create the service principle credentials used to authenticate the client
	credentials = ServicePrincipalCredentials( 
			     client_id = '[service principle client id]',
			     secret = '[service principle secret]',
			     tenant = '[service principle active directory tenant id]')

	# create the client using the created credentials
	client = KeyVaultClient(credentials)

	#retrieve the secret service principle credentials
	secret_bundle = client.get_secret(vault_url, secret_name, secret_version=KeyVaultId.version_none)
	print('getting secret value...')
	secret_value = secret_bundle.value
	print(secret_value)

	#Call the Function API using the retrieved Key Vault secret API KEY
	print('calling api...')

	api_url = "https://benhdemo.azurewebsites.net/api/Pydemo?code=" + secret_value
	data = json.dumps({'name':'Ben H'})
	r = requests.post(api_url, data)

	print('printing secure api response...')
	print(r.text)

```

### 7). Deploy a Python script as a Chron initiated Azure WebJob

Function are a great way for executing your Python scripts on Azure, but an alternate approach is to use a Azure WebJob for running background tasks. You can create Azure WebJobs on any Web App implementation or a Function App that not running in Consumption and have them run continuously or be triggered by a chron interval or run them manually.

Take a Python script in our notebook. 

<img src="https://github.com/ben-houghton/azureforpython/blob/master/images/simplescript.PNG" width="700">


To deploy the Python script as a WebJob, we need to download the notenbook as a Python script locally. 

<img src="https://github.com/ben-houghton/azureforpython/blob/master/images/simplescriptdownload.PNG" width="700">



Then upload the file to the App Service using the Web Job interface. We select the trigger type and then save the Web Job.


<img src="https://github.com/ben-houghton/azureforpython/blob/master/images/webjob.PNG" width="700">


Once deployed the WebJob gets triggered, executes the script and we can view the output in the WebJobs Dashboard.

<img src="https://github.com/ben-houghton/azureforpython/blob/master/images/webjobdashboard.PNG" width="700">






### 8). Exercise: Call the Azure Function API using       

Put the sections we've covered together - 

	- Create a SaaS Jupyter notebook at https://notebooks.azure.com
	- Share the notebook
	- Add the Azure SDK
	- Call the Azure Function Http App using the supplied URL and API Key
			   
 
