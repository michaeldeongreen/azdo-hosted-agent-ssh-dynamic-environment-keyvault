# Overview

This repository contains the Bash scripts that are used in the [TDB](https://blog.michaeldeongreen.com) blog tutorial to demonstrate how to:

* Create a Resurce Group
* Create a Azure Key Vault
* Store a Private Key as a secret in an Azure Key Vault
* Create a Azure Devops Release Pipeline that:
  * Retrieves the Private Key from an Azure Key Vault
  * Installs the Private Key on a Azure DevOps Hosted Agent
  * Uses Public Key Encryption to connect to an Azure Vitual Machine and execute a Docker command