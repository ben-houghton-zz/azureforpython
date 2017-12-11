# Get familiar with the Azure CLI

## Exercise 1 - Create a new Resource Group

1. Open the Azure CLI or Cloud Shell
2. If on the local CLI, login : `az login`
3. Create a new Resource Group (logical resource container): 

    `az group create --name demo --location uksouth`

4. Use the double-tab to test the autocompletion

    `az group create --location <double-tab>`

5. Use the `--no-wait` option on any command to create a more interactive experience

6. Use the `--query` option to filter the results
    
    `az group list --query "[].{ name:name, location:location }" --out tsv`
   
- [] -> projects the object collection
- .{} -> refers to an object in that collection
- KeyValuePairs: existing object property: projected object property

7. Add `grep` to filter the results even further

    `<command> | grep <filter value>`

8. Change the `--output` option to change the default output format. Bonus points: how would you discover what output options exist out of the box?

