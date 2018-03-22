## Use Key Vault to secure API Keys and use this in the Python script

Embedding the API key within the URL is not a particularly secure approach, the key could be easily retrieved from within our source code. The answer to this insecurity is to hold our API Key in Azure Key Vault and to access it securely using REST call and a Service Principle identity.

First thing to do is to create an Azure Key Vault to hold your keys & secrets - https://docs.microsoft.com/en-us/azure/key-vault/key-vault-get-started

There is a great overview for you to follow here - https://docs.microsoft.com/en-us/python/api/overview/azure/key-vault?view=azure-python 

The implementation of this may be spilt into a number of scripts but effectively you need to make a call to Key Vault using a secure Service Principle identity to obtain the API Key and then append the returned value to your API call.

You can build this example in your Notebook

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

