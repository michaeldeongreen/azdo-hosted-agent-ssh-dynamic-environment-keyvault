# Overview

This repository contains the Bash scripts that are used in [this](https://blog.michaeldeongreen.com/post/how-to-ssh-into-an-azure-virtual-machine-using-azure-devops-azure-keyvault-and-bash) blog tutorial to demonstrate how to:

* Create a Resurce Group
* Create a Azure Key Vault
* Store a Private Key as a secret in an Azure Key Vault
* Store an Azure Virtual Machine username as a secret in an Azure Key Vault
* Store an Azure Virtual Machine IP Address as a secret in an Azure Key Vault
* Create a Azure Devops Release Pipeline that:
  * Retrieves secrets from an Azure Key Vault
  * Base64 decodes and installs the Private Key on a Azure DevOps Hosted Agent
  * Uses SSH to connect to an Azure Vitual Machine and deploy the [mcr.microsoft.com/dotnet/core/samples:aspnetapp](https://hub.docker.com/_/microsoft-dotnet-core-samples/) image as a container

## `ssh-key-install.sh`

### Overview

This Bash script uses [azure-cli](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) to retrieve and Base64 decode a Private Key stored as a secret in an Azure Key Vault.

The script will install the Private Key on an Azure DevOps Hosted Agent and set the proper permissions on the Private Key and the directory that contains the Private Key.

The script will add the Private Key to the authentication agent and add the IP Address of the Azure Virtual Machine it will use SSH to connect with in the *known_hosts* directory of the Azure DevOps Hosted Agent.

### Parameters

* [-k] KeyVaultName - Azure Key Vault name
* [-i] VMIPAddress - Virtual Machine IP Address
* [-n] KeyVault Secret Name - Name of the KeyVault Secret

## `aspnetcoreapp-deploy.sh`

This Bash script is executed in an Azure DevOps Release Pipeline Azure CLI Task.  When executed, it calls the `ssh-key-install.sh` script to install a Private Key on an Azure DevOps Hosted Agent.

Once the Private Key has been installed, the script will use [docker -H](https://docs.docker.com/v17.09/engine/reference/commandline/dockerd/) and SSH to connect to an Azure Virtual Machine and deploy the [mcr.microsoft.com/dotnet/core/samples:aspnetapp](https://hub.docker.com/_/microsoft-dotnet-core-samples/) image as a container.

### Parameters

* [-d] Docker Image Name - Docker Image Name
* [-v] Virtual Machine Username - Virtual Machine Username
* [-i] Virtual Machine IP Address - IP Address of Virtual Machine
* [-k] KeyVaultName - Azure Key Vault name
* [-d] Key Install Script Directory - Directory where the `ssh-key-install.sh` script resides