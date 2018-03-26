

### Deploy a Python script as a Chron initiated Azure WebJob

Function are a great way for executing your Python scripts on Azure, but an alternate approach is to use a Azure WebJob for running background tasks. You can create Azure WebJobs on any Web App implementation or a Function App that not running in Consumption and have them run continuously or be triggered by a chron interval or run them manually.

Thee's a good overview here https://docs.microsoft.com/en-us/Azure/app-service/web-sites-create-web-jobs

Take a Python script in our notebook. 

<img src="https://github.com/ben-houghton/azureforpython/blob/master/images/simplescript.PNG" width="700">


To deploy the Python script as a WebJob, we need to download the notebook as a Python script locally. 

<img src="https://github.com/ben-houghton/azureforpython/blob/master/images/simplescriptdownload.PNG" width="700">



Then upload the file to the App Service using the Web Job interface. We select the trigger type and then save the Web Job.


<img src="https://github.com/ben-houghton/azureforpython/blob/master/images/webjob.PNG" width="700">


Once deployed the WebJob gets triggered, executes the script and we can view the output in the WebJobs Dashboard.

<img src="https://github.com/ben-houghton/azureforpython/blob/master/images/webjobdashboard.PNG" width="700">

