Azure Kubernetes Service
=========

A role help to create Kuberntes Service in Azure.

Requirements
------------

The role uses Ansible azure modules, and miniest supported version is `2.8.0`.
> Getting started with Ansible Azure modules with [Microsoft Docs](https://docs.microsoft.com/en-us/azure/ansible/ansible-overview)

Role Variables
--------------

| variable | Required | Default Value | description |
|--|--|--|--|
| name | yes |  | Name of the Kubernetes Service resource |
| resource_group | yes | | Resource group of the resource |
| aad_client_app_id | | | The ID of an Azure Active Directory client application of type `Native`. <br/>This application is for user login via kubectl.|
| aad_server_app_id | | | The ID of an Azure Active Directory server application of type `Web app/API`. <br/>This application represents the managed cluster's apiserver (Server application). |
| aad_server_app_secret | | | The secret of an Azure Active Directory server application. |
| aad_tenant_id | | | The ID of an Azure Active Directory tenant. |
| admin_username | | azureuser | User account to create on node VMs for SSH access. |
| agent_pool_type | | AvailabilitySet | Possible values include VirtualMachineScaleSets and AvailabilitySet. |
| service_principal | | Loading from ansible-playbook, environment variable `AZURE_CLIENT_ID` or `~/.azure/credentials` | Service principal used for authentication to Azure APIs. |
| client_secret | | Loading from ansible-playbook, environment variable `AZURE_SECRET` or `~/.azure/credentials` | Secret associated with the service principal. |
| dns_prefix | | The same as `name` | Prefix for hostnames that are created. |
| dns_service_ip | | | An IP address assigned to the Kubernetes DNS service. <br/>*This address must be within the Kubernetes service address range specified by `service_cidr`.* |
| docker_bridge_cidr | | | A specific IP address and netmask for the Docker bridge, using standard CIDR notation.<br/> *This address must not be in any Subnet IP ranges, or the Kubernetes service address range.* |
| enable_rbac | | True | Enable Kubernetes Role-Based Access Control. |
| http_application_routing | | False | Enable `http_application_routing` addon. Configure ingress with automatic public DNS name creation. |
| kubernetes_version | | First value from `azure_rm_aks_version` module |  Version of Kubernetes to use for creating the cluster. |
| load_balancer_sku | | Basic |   The load balancer sku for the managed cluster. Standard or Basic |
| location | | eastus | Region of the Kubernetes Service resource, will use `resource_group`'s location if not specified. <br/>*Location is required if resource group not exist*|
| max_pods | | 110| The maximum number of pods deployable to a node. |
| monitoring | | False | Enable `monitoring` addon. Turn on Log Analytics monitoring. |
| network_plugin | | Choices:<br/>&nbsp;- **kubenet**<br/>&nbsp;- azure | The Kubernetes network plugin to use. |
| network_policy | | | The Kubernetes network policy to use. Using together with "azure" network plugin. Specify `azure` for Azure network policy manager and `calico` for calico network policy controller. |
| node_count | | 3 | Number of nodes in the Kubernetes node pool. |
| node_osdisk_size_gb | | 30 | Size in GB of the OS disk for each node in the node pool. |
| node_vm_size | | Standard_DS1_v2 | Size of Virtual Machines to create as Kubernetes nodes. |
| nodepool_name | | nodepool1 | Node pool name, upto 12 alphanumeric characters. |
| os_type | | Linux | |
| pod_cidr | | |  A CIDR notation IP range from which to assign pod IPs when kubenet is used. <br/>*This range must not overlap with any Subnet IP ranges.* |
| resource_tags | | | Dictionary of resource tags.  |
| service_cidr | | | A CIDR notation IP range from which to assign service cluster IPs. <br/>*This range must not overlap with any Subnet IP ranges.* |
| storage_profile | | ManagedDisks | |
| ssh_key | | Loading from `~/.ssh/id_rsa.pub` | Public key path or key contents to install on node VMs for SSH access. |
| virtual_node | | False | Enable `virtual_node` addon. Fast provisioning of pods with Azure Container Instance.  |
| virtual_node_subnet_id | | Create a new resource when `virtual_node` is `True`. | |
| vnet_subnet_id | | Create a new resource when `virtual_node` is `True` or `network_plugin` defined. | The ID of a subnet in an existing VNet into which to deploy the cluster. |
| workspace_resource_id | | Use the first Log Analytics Workspace in the `resource_group` or create a new resource when `monitoring` is `True`. | The resource ID of an existing Log Analytics Workspace to use for storing monitoring data. |


Example Playbook
----------------
Install the role via:

```bash
ansible-galaxy install azure.aks
```

Use the role in the playbook to create the most default AKS:

```yml
- hosts: localhost
  tasks:
      - include_role:
           name: azure.aks
        vars:
           name: akscluster
           resource_group: aksroletest
```
Create an AKS with monitoring:

```yml
- hosts: localhost
  tasks:
      - include_role:
           name: azure.aks
        vars:
           monitoring: yes
           name: akscluster
           resource_group: aksroletest
```
Use of Resource Tags

```yml
- hosts: localhost
  tasks:
      - include_role:
           name: azure.aks
        vars:
           name: akscluster
           resource_group: aksroletest
           resource_tags:
              'service name': 'akscluster'
              'service location': "{{ location }}"
            
```


License
-------

MIT
