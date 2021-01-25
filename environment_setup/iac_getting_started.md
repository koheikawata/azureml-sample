# Getting Started with MLOpsPython <!-- omit in toc -->

This guide shows how to deploy and maintain Azure resources by codes. This project uses Azure Resource Manager Template through Azure Pipelines on Azure DevOps. You can follow this step by step guide when you are in the beginning of the project.

- [Setting up Azure DevOps](#setting-up-azure-devops)
- [Get the code](#get-the-code)
- [Create an Azure DevOps Service Connection for the Azure Resource Manager](#create-an-azure-devops-service-connection-for-the-azure-resource-manager)
- [Create a Variable Group for your Pipeline](#create-a-variable-group-for-your-pipeline)
- [Create the IaC Pipeline](#create-the-iac-pipeline)
- [Provisioning resources using Azure Pipelines](#provisioning-resources-using-azure-pipelines)

## Setting up Azure DevOps

We recommend using Azure Pipelines on Azure DevOps as a deployment pipeline, where Azure resources are deployed through ARM templates. If you don't already have an Azure DevOps organization, create one by following the instructions at [Quickstart: Create an organization or project collection](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/create-organization?view=azure-devops).

## Get the code

You can use either Azure DevOps repository or Github repository for your codes. We recommend using the [repository template](), which effectively forks the repository to your own repository location and squashes the history. You can use the resulting repository for this guide and for your own experimentation.

## Create an Azure DevOps Service Connection for the Azure Resource Manager

The IaC provisioning pipeline requires an Azure Resource Manager service connection. On your Azure DevOps, navigate to Project settings -> Service connections -> New srvice connection -> Azure Resource Manager -> Service Principal (automatic). Set up the service connection name, which you need to set up in the variable group next step.

## Create a Variable Group for your Pipeline

This IaC pipeline requires some variables to be set before you can run the pipeline. You'll need to create a variable group in Azure DevOps to store values that are reused across multiple pipelines or pipeline stages. Either store the values directly in Azure DevOps or connect to an Azure Key Vault in your subscription. Check out the Add & use variable groups documentation to learn more about how to create a variable group and link it to your pipeline.

Navigate to Library in the Pipelines section, and then create a variable group named **`vg-azure-sample`**. The YAML pipeline definitions in this repository refer to this variable group by name. The variable group should contain the following required variables. It is recommended you follow [Cloud Adoption Framework naming convention](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/resource-naming) for your Azure Resources. Here, you can only define **`BASE_NAME`**, and the names of Azure Machine Learning Service Workspace, Key Vault, Storage Account, Container Registry, Application Insights are defined following the naming convention.

| Variable Name            | Suggested Value           | Short description                                                                                                           |
| ------------------------ | ------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| BASE_NAME                | [your project name]       | Unique naming prefix for created resources - max 10 chars, letters and numbers only                                         |
| LOCATION                 | japaneast                 | [Azure location](https://azure.microsoft.com/en-us/global-infrastructure/locations/), no spaces                             |
| RESOURCE_GROUP           | rg-mlops                  | Azure Resource Group name                                                                                                   |
| AZURE_RM_SVC_CONNECTION  | armsvc                    | [Azure Resource Manager Service Connection](#create-an-azure-devops-service-connection-for-the-azure-resource-manager) name |

## Create the IaC Pipeline

In your Azure DevOps project, create a build pipeline from your forked repository. Select the **Existing Azure Pipelines YAML file** option and set the path to [/environment_setup/iac-azuredeploy-ado.yml](./iac-azuredeploy-ado.yml).

## Provisioning resources using Azure Pipelines

Having done that, run the pipeline, and then check that the newly created resources appear in the [Azure Portal](https://portal.azure.com).
