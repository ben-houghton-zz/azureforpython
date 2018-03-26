

# Build an Azure API in Python that performs sentiment analysis on a corpus of text

Take a look at Azure Cognitive Services - these are Artificial Intelligence Software as a Service applications.

https://azure.microsoft.com/en-us/services/cognitive-services/directory/

and

https://azure.microsoft.com/en-us/services/cognitive-services/text-analytics/

and 

https://azure.microsoft.com/en-us/services/cognitive-services/emotion/


In this exercise, we are going to build  a new Python Function in your serverless Function App.

This Python Function code receives a collection of text snippets as a POST body which it submits to Azure Cognitive Services Text Analytics API. The Function makes a REST call with Post body to the API using the HTTPLib module. This is a great example of how to very quickly build an API in Azure. You can use this approach to build multiple APIs that you can expose publically or keep private.

In the Azure Portal create your Function App as a HttpTrigger using the 'Experimental Language' of Python. Use the text below as the body of the Function.

**You'll need to obtain a Subscription Key for the Text Analytics Cognitive Services API before running this example**

```
import os
import json
import httplib, urllib, base64

postreqdata = json.loads(open(os.environ['req']).read())

# Request headers
headers = {

    'Content-Type': 'application/json',
    'Ocp-Apim-Subscription-Key': '[API Subscription Key]',
}

params = urllib.urlencode({
})

#Encode request body
postdata = json.dumps(postreqdata).encode('utf-8')

#Create API request
conn = httplib.HTTPSConnection('westus.api.cognitive.microsoft.com')
conn.request("POST", "/text/analytics/v2.0/sentiment?%s" % params, postdata, headers)
response = conn.getresponse()
data = response.read()
print(data)
conn.close()

#Return response
response = open(os.environ['res'], 'w')
response.write(data)
response.close()
```

You can make a Post request to your Sentiment Analysis Function through using either POSTMAN or your Python Notebook using the following JSON body. You need to set the request content type to 'application/json'.

```
{
        "documents": [
            {
                "language": "en",
                "id": "1",
                "text": "We love this trail and make the trip every year. The views are breathtaking and well worth the hike!"
            },
            {
                "language": "en",
                "id": "2",
                "text": "Poorly marked trails! I thought we were goners. Worst hike ever."
            },
            {
                "language": "en",
                "id": "3",
                "text": "Everyone in my family liked the trail but thought it was too challenging for the less athletic among us. Not necessarily recommended for small children."
            },
            {
                "language": "en",
                "id": "4",
                "text": "It was foggy so we missed the spectacular views, but the trail was ok. Worth checking out if you are in the area."
            },                
            {
                "language": "en",
                "id": "5",
                "text": "This is my favorite trail. It has beautiful views and many places to stop and rest"
            },
            {
                "language": "es", 
                "id": "6", 
                "text": "Los caminos que llevan hasta Monte Rainier son espectaculares y hermosos."
            }
        ]
    }
```
Your POSTMAN request should look like this - 

 <img src="https://github.com/ben-houghton/azureforpython/blob/master/images/postmanrequest.PNG" width="700">
 
 You can change the API call to use the Keyword Analysis service by replacing 'sentiment' with 'keyPhrases' - have a try!
 
 You now have a fully functioning API that you have built that you can use to determine the sentiment analysis or keywords of a corpus of text.
 
### Now build some code in your Azure Notebook to call the API you've built with a payload of text to analyse
