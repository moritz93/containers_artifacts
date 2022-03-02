# Enabling ADD 
az aks update -g teamResources -n aksteam4 --enable-aad

# Retrieve cluster ID
AKS_ID=$(az aks show -g teamResources -n aksteam4 --query id -o tsv)