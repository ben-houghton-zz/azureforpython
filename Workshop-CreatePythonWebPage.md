# Build a Storage Account based Python Website

In this exercise we will build a simple Python based website that is hosted in an Azure Storage Account rather than a PaaS Web App

You can do this either from notebooks.azure.com, or from your Python development environment.

To make this script work, you'll need your storage account name and key.

```

import os, uuid, sys

from azure.storage.blob import BlockBlobService, PublicAccess, ContentSettings

# Create the BlockBlockService that is used to call the Blob service for the storage account

block_blob_service = BlockBlobService(account_name='[storage_account_name]', account_key='[storage_account_key]')


# Create a container called 'quickstartblobs'.

container_name ='mycontainername'

block_blob_service.create_container(container_name)


# Set the permission so the blobs are public.

block_blob_service.set_container_acl(container_name, public_access=PublicAccess.Container)

# Create a web page blob
block_blob_service.create_blob_from_bytes(
    'mycontainername',
    'myblobname',
    b'<center><h1>Hello World!</h1></center>',
    content_settings=ContentSettings('text/html')
)

print(block_blob_service.make_blob_url('mycontainername', 'myblobname'))

```
