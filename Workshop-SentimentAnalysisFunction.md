
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
