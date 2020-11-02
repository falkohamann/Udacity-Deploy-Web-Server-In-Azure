# Azure Infrastructure Operations Project: Deploying a scalable IaaS web server in Azure

## Introduction

For this project, you will write a Packer template and a Terraform template to deploy a customizable, scalable web server in Azure.

## Getting Started

1. Clone this repository
2. Create your infrastructure as code
3. Create your tagging-policy in Azure

## Dependencies

1. Create an [Azure Account](https://portal.azure.com) 
2. Install the [Azure command line interface](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)
3. Install [Packer](https://www.packer.io/downloads)
4. Install [Terraform](https://www.terraform.io/downloads.html)

## Instructions

Once you've  collected your dependencies, to deploy the scalable web server in Azure:

1. Deploy the packer image
2. Deploy the infrastructure with Terraform template

### 1. Deploy the Packer Image

Packer is a server templating software. It will deploy virtual machines images. After deploying the virtual machines with the help of packer template, make sure to delete the packer images as it does not maintain the state.

- [] Config Environment Variables

Go to the terminal and export the environment variables like below.

```bash
export ARM_CLIENT_ID=4685768f-1912-4c9a-8226-b670918xxxxfakeclientid
export ARM_CLIENT_SECRET=6GNB5c5p_5H.-odi_zffakesecret
export ARM_SUBSCRIPTION_ID=59ce2236-a139-4c5fakesubsribtionid
```

#### Get Subscription ID

* Login into your azure account
* Search and click "Subscriptions"
* Select whichever subscriptions are needed
* Click on the overview
* Copy the Subscription Id

#### Get Client ID

* Login into your azure account
* Search and click "Azure Active Directory"
* Click "App registrations" under the Manage tab
* Click the application that you own
* Copy the client ID

#### Get Client Secret

* Login into your azure account
* Search and click "Azure Active Directory"
* Click "App registrations" under the Manage tab
* Click the application that you own
* Click the "Certificates & Secrets" under the Manage tab
* Create a client secret as you need.

Once you have exported and config the environment variable, use <b> printenv</b> to check whether they are configured properly.

```bash
printenv
```

- [] Deploy the Packer Image

```bash
packer build server.json
```

## 2. Create and Update Azure Resouces with Terraform Template

Now we come to deploy the resources using the Terraform template. One thing worth mentioning is that we have already created the resources group for our PackerImage, so we can't deploy the resource group with the same name. Instead, we need to import the existing resource group and then it will know which resource group to deploy. The similar command will be like:

```bash
terraform import azurerm_resource_group.main /subscriptions/{subsriptionId}/resourceGroups/{resourceGroupName}
```

> In main.tf: The az availability set, platform_fault_domain_count = 2 has default value 5, so we need to specify it to 2.

### Deploy the Infrastructure

Run the following command to deploy the infrastructure.

```bash
terraform plan -out solution.plan
terraform apply
```

Once you have deployed the infrastructure. You can go to the Azure portal to check the resources. Once you have finished, remember to destroy these resources.

```bash
terraform destroy
```

### Specify the Variables

To use variables for your main.tf, you can specify your variables like below in your vars.tf file.

```tf
variable "environment"{
  description = "The environment should be used for all resources in this example"
  default = "test"
}
```

And in your main.tf, you can call the variables like

```tf
var.environment
```

## Output