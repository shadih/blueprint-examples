# Hello World Example

This repository contains several Hello World Application Examples.

If you're only now starting to work with Cloudify see our [Getting Started Guide](https://cloudify.co/getting-started/).

This document will guide you how to run the examples step by step.


## Using the CLI


### Prepering the environment

Download the example
```shell
curl -L https://github.com/cloudify-cosmo/cloudify-hello-world-example/archive/master.zip -o cloudify-hello-world-example.zip
i think its the wrong link!! fix that 
```

Extract the example
```shell
unzip cloudify-hello-world-example.zip && cd cloudify-hello-world-example-master
```

Install Cloudify plugins
```shell
cfy plugins bundle-upload
```


### Creating secrets

Create secrets according to your IaaS provider.

Replace <value> with actual values, without the <>

For **AWS**
```shell
cfy secrets create aws_access_key_id --secret-string <value>
cfy secrets create aws_secret_access_key --secret-string <value>
```
 
For **Azure**
```shell
cfy secrets create azure_subscription_id --secret-string <value>
cfy secrets create azure_tenant_id --secret-string <value>
cfy secrets create azure_client_id --secret-string <value>
cfy secrets create azure_client_secret --secret-string <value>
```

For **GCP**

gcp_credentials: A GCP service account key in JSON format. **Hint: Create this secret from a file:**
```shell   
`cfy secrets create gcp_credentials -f ./path/to/JSON key`.
```                                             
gcp_zone: A GCP Zone such as `us-east1-b`:                                                              

```shell
cfy secrets create gcp_zone --secret-string <zone>                                                                                                                                              
```

For **Openstack**
```shell
cfy secrets create openstack_username --secret-string <value>
cfy secrets create openstack_password --secret-string <value>
cfy secrets create openstack_tenant_name --secret-string <value>
cfy secrets create openstack_auth_url --secret-string <value>
```

**Notes:** 

1. Use v3 authentication url.

2. If v2 authentication url used, remove user_domain_name and project_domain_name
from client_config. 
                  
         
### Running the example


For **AWS**:

```shell
cfy install aws.yaml 
```
Or 
```shell
cfy install aws.yaml -i aws_region_name=<region_name>
```
If region not provided the default region will be used(see inputs section of aws blueprint). 

For **Azure**:

```shell
cfy install azure.yaml -i location=eastus -i agent_password=OpenS3sVm3
```
If location input not provided the default location will be "eastus2".

For **GCP**:

```shell
cfy install gcp.yaml -i region=<region> -i zone=<zone>
```
Or 
```shell
cfy install gcp.yaml 
```
If region and zone input not provided the default values will be used(see gcp.yaml inputs section).

For **Openstack**:

```shell 
cfy install openstack.yaml \
-i region=RegionOne \
-i external_network_id=dda079ce-12cf-4309-879a-8e67aec94de4 \
-i image=e41430f7-9131-495b-927f-e7dc4b8994c8 -i flavor=m1.small
```
In case that project_domain_name, project_domain_name is not "default" please provide them as inputs.

###Get deployment id:       
```shell
cfy deployments list         
```
###Get the URL of the webserver
```shell
cfy deployment outputs <deployment_id>
```
## Using the Web UI

### Open the Web UI

1. Open the browser with the Cloudify Manager's public IP provided during installation
2. Login with user 'admin', password can be either:
    * Using one of the images (AMI, QCOW, Docker), password is 'admin'
    * Password provided during installation (in config.yaml)
    * Password generated during installation and printed to screen

### Install Cloudify plugins

1. Go to 'Cloudify Catalog' on the left side menu
2. In the 'Plugins Catalog' widget, select the plugin of the IaaS of your choice and click 'install' button on the right side

### Creating secrets

1. Go to 'System Resources on the left side menu'
2. Scroll down to the 'Secret Store Management' widget
3. Create secrets using the 'Create' button and according to your IaaS provider:
('Secret key' according to the list below and 'Secret value' with your specific values)
    * For **AWS**
        * aws_access_key_id
        * aws_secret_access_key
    * For **Azure**
        * subscription_id
        * tenant_id
        * client_id
        * client_secret
    * For **GCP**
        * gcp_credentials(service account JSON file)
    * For **Openstack**
         * openstack_username
         * openstack_password
         * openstack_tenant_name
         * openstack_auth_url
 

### Running the example

1. Go to 'Local Blueprints' menu and click 'Upload' button
2. In the blueprint URL filed put https://github.com/cloudify-cosmo/cloudify-hello-world-example/archive/master.zip
3. In the blueprint YAML file select one of the following  
    * azure.yaml
    * openstack.yaml
    * aws.yaml
    * gcp.yaml
4. Click 'upload'
5. In the blueprints table, click the 'cloudify-hello-world-example-master' link
6. Click 'Create Deployment' button
7. Type 'cloudify-hello-world-example-master' in the deployment name field
8. Complete the inputs' values:
    * For **AWS**
        * aws_region_name, for example 'eu-central-1'
    * For **Azure**
        * location, for example 'eastus'
        * agent_password, for example 'OpenS3sVm3'
    * For **GCP**
        * region, for example 'europe-west1'
    * For **Openstack**
         * region, for example 'RegionOne'
         * external_network_id, for example the id of 'GATEWAY_NET'
         * image, for example '05bb3a46-ca32-4032-bedd-8d7ebd5c8100'
         * flavor, for example 'm1.small'
9. Click 'Deploy'
10. Scroll down to the deployments table, click the hanburger menu on the right and select 'Install'

