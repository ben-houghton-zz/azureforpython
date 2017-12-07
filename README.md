

# Azure for Python Developers

 In this workshop, we are going to - 

			1. Create a Jupyter Notebook in notebooks.azure.com
			2. Share the notebook with users in the workshop
			3. Install Azure SDK to notebook
			4. Show how to create Azure resources in notebook using Azure SDK
			5. Show how to call a Function API in Python in the notebook using Azure SDK
			6. Show how to call a Function API as a REST service in Python & notebook
			7. Show how to use Key Vault to secure API Keys and use this in the Python script
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


The best place to start with help for building Python Apps that run on or integrate with Azure is the Azure Python Developer Centerhttps://azure.microsoft.com/en-us/develop/python/. You can find lots of examples and development scenarios the and also the Azure Python SDK reference http://azure-sdk-for-python.readthedocs.io/en/latest/

### 4). Create Azure Resources using the Azure Python SDK

There are some good examples of how to integrate Python and the Azure SDK. This example https://github.com/Azure-Samples/storage-blobs-python-quickstart demonstates how to use the Azure SDK to add files to a storage account. 

You'll first need to install the Azure Storage SDK

```javascript
	pip install azure-storage-blob
```

If you follow the instructions to create a storage account through the Azure Portal and then run the script using you storage account name and keys you see the script execute.

You'll need to make one change to the script. It looks for a relative directory that doesn't exist, so simply just remove the reference to 'Documents'.

Before -
<img src="https://github.com/ben-houghton/azureforpython/blob/master/images/scripteditorig.PNG" width="700">


After -
<img src="https://github.com/ben-houghton/azureforpython/blob/master/images/scriptedit.PNG" width="700">


When the script runs in your notebook, it's create a new Blb Container and add a file.

<img src="https://github.com/ben-houghton/azureforpython/blob/master/images/scriptrun.PNG" width="700">

### 5). Call a Function API in Python in the notebook using Azure SDK


### 6). Call a Function API as a REST service in Python & notebook
