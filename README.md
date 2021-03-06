Build Status: ![alt text](https://anpohl.visualstudio.com/_apis/public/build/definitions/a783ad3e-63dc-4fea-b0fd-79f07db8d534/1/badge "Build Status")

# SFChatServer
Chat Server Based on Service Fabric

#Deployment
Information for Deployment

##Key Vault
Using KeyVault as Secure Password provider for Deployment.

Open Powershell and Login to your Azure Account
```powershell
Login-AzureRmAccount
```

Switch context to the Subscription you want to use for your KeyVault
```powershell
Set-AzureRmContext -SubscriptionId <subscription-id>
```

If you dont allready have a Ressourcegroup you want to User for your KeyVault create a new one
```powershell
New-AzureRmResourceGroup -Name '<ResourceGroupName>' -Location 'westeurope'
```

Now we are able to create a new KeyVault in Azure
```powershell
New-AzureRmKeyVault -VaultName '<KeyVaultName>' -ResourceGroupName '<ResourceGroupName>' -Location 'westeurope'
```

Generate a new Secrete that contains your Password and add it to your KeyVault
```powershell
$secretvalue = ConvertTo-SecureString '<PASSWORD>' -AsPlainText -Force
$secret = Set-AzureKeyVaultSecret -VaultName '<KeyVaultName>' -Name '<passwordName>' -SecretValue $secretvalue
```

Now we need to Enable the KeyVault for Deployment. 
```powershell
Set-AzureRmKeyVaultAccessPolicy -EnabledForDeployment -EnabledForTemplateDeployment -ResourceGroupName <ResourceGroupName>
```

Deploy the Service Fabric Chat Cluster
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName <ResourceGroupNameForCluster> -TemplateFile https://raw.githubusercontent.com/Annonator/SFChatServer/master/Deployment/ServiceFabricClusterTemplate.json -TemplateParameterFile "<LocalPathTo>\Parameter.json"
```

