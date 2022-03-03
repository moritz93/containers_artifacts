# Commands

## Enabling ADD 

az aks update -g teamResources -n aksteam4 --enable-aad

## Retrieve cluster ID

AKS_ID=$(az aks show -g teamResources -n aksteam4 --query id -o tsv)

## Create the keyvault

az keyvault create -n <keyvault-name> -g RG -l westeurope

# Create the SQL secret into the keyvault

az keyvault secret set --vault-name <keyvault-name> -n ExampleSecret --value MyAKSExampleSecret

```json
"addonProfiles": {
      "azureKeyvaultSecretsProvider": {
        "config": null,
        "enabled": true,
        "identity": {
          "clientId": "90099273-4047-4acb-9d78-851575b7ba16",
          "objectId": "00b2fb59-5f7e-4a0b-bb01-c4e04132286a",
          "resourceId": "/subscriptions/e153a3a9-69d5-4566-86db-5037d2b72ae2/resourcegroups/MC_teamResources_aksteam4_northeurope/providers/Microsoft.ManagedIdentity/userAssignedIdentities/azurekeyvaultsecretsprovider-aksteam4"
        }
      }
```

## Assign permissions on the keyvault to the cluster identity

```shell
# set policy to access keys in your key vault
az keyvault set-policy -n <keyvault-name> --key-permissions get --spn <identity-client-id>
# set policy to access secrets in your key vault
az keyvault set-policy -n <keyvault-name> --secret-permissions get --spn <identity-client-id>
# set policy to access certs in your key vault
az keyvault set-policy -n <keyvault-name> --certificate-permissions get --spn <identity-client-id>
```
