# Create a Python HttpTrigger that performs sentiment analysis on a collection text

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

Post Request JSON Body
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
